+++
title = "Creating the New Post Lambda Function"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++

In this task, we will create the first Lambda function of the application. This function serves as the entry point, receiving information about new posts that need to be converted into audio files.

## Steps to Create the Lambda Function:

**Step 1.** Navigate to the AWS Management Console and search for Lambda.

**Step 2.** On the Lambda service page, select **Create function**.

**Step 3.** Set up the function.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.016.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.017.png)

- Choose the option **Author from scratch**.
- **Function name:** Enter **PostReader_NewPost**.
- **Runtime:** Select **Python 3.12**.
- Click on **Change default execution role** to expand the role options.

    - **Execution role:** Select **Use an existing role**.
    - **Existing role:** Choose **Lab-Lambda-Role**.

- Select **Create function** to complete the setup.

**Step 4.** Add code to the function.

In the **Function code** section, delete any existing code.

Paste the following Python code:
```python
import boto3
import os
import uuid

def lambda_handler(event, context):

    recordId = str(uuid.uuid4())
    voice = event["voice"]
    text = event["text"]

    print('Generating new DynamoDB record, with ID: ' + recordId)
    print('Input Text: ' + text)
    print('Selected voice: ' + voice)

    # Create a new record in the DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    table.put_item(
        Item={
            'id': recordId,
            'text': text,
            'voice': voice,
            'status': 'PROCESSING'
        }
    )

    # Send notification about the new post to SNS
    client = boto3.client('sns')
    client.publish(
        TopicArn=os.environ['SNS_TOPIC'],
        Message=recordId
    )

    return recordId
```

## The Lambda function performs the following tasks:

- Takes two input parameters:
    + **Voice:** One of the many voices supported by Amazon Polly.
    + **Text:** The content of the post that needs to be converted into an audio file.

- Creates a new record in the DynamoDB table with information about the new post.

- Sends information about the new post to SNS.

- Returns the ID of the DynamoDB item to the user.

**Step 5.** Click **Deploy** to save the changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.018.png)

**Step 6.** Configure environment variables.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.019.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.020.png)

Navigate to the **Configuration** tab.

In the left panel, select **Environment variables**.

Click on **Edit** and add the following variables:

+ **Key:** Enter **SNS_TOPIC**, **Value:** [Paste the ARN of the SNS topic you created earlier].
+ **Key:** Enter **DB_TABLE_NAME**, **Value:** Enter **posts**.

Click on **Save**.

**Step 7.** General configuration.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.021.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.022.png)

- In the left panel of the **Configuration** tab, select **General configuration**.

- Click on **Edit**.

- Update **Timeout** to **10 seconds**.

- Click on **Save**.

## Steps to test the Lambda function:

**Step 1.** Create a test event.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.023.png)

Navigate to the **Test** tab.

Configure a new test event with the following details:

+ **Event name:** Joanna
+ **Event JSON:**

```
{
  "voice": "Joanna",
  "text": "This is working!"
}
```
+ Click **Save**.

**Step 2.** Run the test.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.024.png)

Click on **Test** to execute your test event.

You will see the message “Execution result: succeeded.”

Expand the **Details** section to view the execution logs.
