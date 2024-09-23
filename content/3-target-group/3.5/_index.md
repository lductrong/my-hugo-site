## Creating the “Convert to Audio” Lambda Function

In this task, we will create a Lambda function to convert text stored in the DynamoDB table into an audio file. This is the central function of the application that defines its primary functionality.

## Steps to Create the Lambda Function

**Step 1.** Navigate to the AWS Management Console and search for Lambda.

**Step 2.** In the Lambda service page, select **Create function**.

**Step 3.** Configure the function.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.025.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.026.png)

- Select the option **Author from scratch**.

- **Function name:** Name the function **ConvertToAudio**.

- **Runtime:** Select **Python 3.12**.

- Click on **Change default execution role** to expand the default execution role options.

    + **Execution role:** Select **Use an existing role**.

    + **Existing role:** Choose **Lab-Lambda-Role**.

- Select **Create function** to complete the function creation process.

**Step 4.** Add code to the function.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.027.png)

In the **Function code** section, delete the existing code.

Paste the following Python code:

``` python
import boto3
import os
from contextlib import closing
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["Records"][0]["Sns"]["Message"]

    print ("Text to Speech function. Post ID in DynamoDB: " + postId)

    # Retrieving information about the post from DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    postItem = table.query(
        KeyConditionExpression=Key('id').eq(postId)
    )


    text = postItem["Items"][0]["text"]
    voice = postItem["Items"][0]["voice"]

    rest = text

    # Because single invocation of the polly synthesize_speech api can
    # transform text with about 3000 characters, we are dividing the
    # post into blocks of approximately 2500 characters.
    textBlocks = []
    while (len(rest) > 2600):
        begin = 0
        end = rest.find(".", 2500)

        if (end == -1):
            end = rest.find(" ", 2500)

        textBlock = rest[begin:end]
        rest = rest[end:]
        textBlocks.append(textBlock)
    textBlocks.append(rest)

    # For each block, invoke Polly API, which transforms text into audio
    polly = boto3.client('polly')
    for textBlock in textBlocks:
        response = polly.synthesize_speech(
            OutputFormat='mp3',
            Text = textBlock,
            VoiceId = voice
        )

        # Save the audio stream returned by Amazon Polly on Lambda's temp
        # directory. If there are multiple text blocks, the audio stream
        # is combined into a single file.
        if "AudioStream" in response:
            with closing(response["AudioStream"]) as stream:
                output = os.path.join("/tmp/", postId)
                if os.path.isfile(output):
                    mode = "ab"  # Append binary mode
                else:
                    mode = "wb"  # Write binary mode (create a new file)
                with open(output, mode) as file:
                    file.write(stream.read())

    s3 = boto3.client('s3')
    s3.upload_file('/tmp/' + postId,
      os.environ['BUCKET_NAME'],
      postId + ".mp3")
    s3.put_object_acl(ACL='public-read',
      Bucket=os.environ['BUCKET_NAME'],
      Key= postId + ".mp3")

    location = s3.get_bucket_location(Bucket=os.environ['BUCKET_NAME'])
    region = location['LocationConstraint']

    if region is None:
        url_beginning = "https://s3.amazonaws.com/"
    else:
        url_beginning = "https://s3-" + str(region) + ".amazonaws.com/"

    url = url_beginning \
            + str(os.environ['BUCKET_NAME']) \
            + "/" \
            + str(postId) \
            + ".mp3"

    # Updating the item in DynamoDB
    response = table.update_item(
        Key={'id':postId},
          UpdateExpression=
            "SET #statusAtt = :statusValue, #urlAtt = :urlValue",
          ExpressionAttributeValues=
            {':statusValue': 'UPDATED', ':urlValue': url},
        ExpressionAttributeNames=
          {'#statusAtt': 'status', '#urlAtt': 'url'},
    )

    return
```
## Lambda Function Responsibilities

The Lambda function performs the following tasks:

+ Retrieves the text content from DynamoDB based on the provided post ID.
+ Splits the text into smaller segments (if necessary).
+ Uses Amazon Polly to convert each text segment into speech.
+ Saves the resulting audio files to an S3 bucket.

Splitting the text into segments before converting it to audio and then concatenating the audio segments helps avoid Polly's input limit of 3,000 characters. This makes the application run smoothly and efficiently.

**Step 5.** Click **Deploy** to save the changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.028.png)

**Step 6.** Configure environment variables.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.029.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.030.png)

Navigate to the **Configuration** tab.

In the left-hand panel, select **Environment variables**.

Click **Edit** and add the following variables:

+ **Key:** Enter: `DB_TABLE_NAME`, **Value:** Enter: `posts`
+ **Key:** Enter: `BUCKET_NAME`, **Value:** Enter: `audioposts-19012003` (the name of the bucket you created earlier).

Click **Save**.

**Step 7.** General configuration.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.031.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.032.png)

- In the left-hand panel of the **Configuration** tab, select **General configuration**.

- Click **Edit**.

- Update the **Timeout** to **5 minutes**.

- Click **Save**.

**Step 8.** Set up the function to trigger automatically when a message is sent to the SNS topic created earlier.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.033.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.034.png)

- In the left-hand panel of the **Configuration** tab, select **Triggers**.

- Click **Add trigger**.

- Choose the source as **SNS**.

- **SNS topics:** Select `new_posts` from the available topics.

- Click **Add**.

Now we have created two Lambda functions. Next, we can test whether the two Lambda functions communicate successfully through SNS.
