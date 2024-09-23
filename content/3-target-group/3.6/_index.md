## Testing the Functions

In this task, we will test whether the two created Lambda functions are working correctly. The operational workflow includes:

1. Manually triggering the New Post Lambda function.
2. Data is added to DynamoDB and a message is sent to the SNS topic.
3. SNS triggers the Convert to Audio function.
4. The Convert to Audio function uses Amazon Polly to create an audio file.
5. The audio file is saved in an S3 bucket.

## Perform the Test:

#### 1. Manually Trigger the New Post Lambda Function

**Step 1.** Navigate to the AWS Lambda service page.

**Step 2.** In the left menu, select **Functions**.

Select the function `PostReader_NewPost`.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.035.png)

**Step 3.** Switch to the **Test** tab and then click **Test**.

A message will appear when the function executes successfully.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.036.png)

#### 2. Check if Data Was Added to DynamoDB

**Step 1.** Navigate to the AWS DynamoDB service page.

**Step 2.** In the left menu, select **Explore items**.

**Step 3.** Select the table **posts**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.037.png)

Depending on how many times you have tested the New Post function, the number of items in the posts table will vary.

#### 3. Check the Activity of the Convert to Audio Function

**Step 1.** Navigate to the AWS Lambda service page.

**Step 2.** In the left menu, select **Functions** and choose the function **ConvertToAudio**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.038.png)

**Step 3.** Click on the **Monitor** tab.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.039.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.040.png)

You can view the monitoring charts for the function calls. In the **Recent invocations** section, you should see two recent calls corresponding to your New Post function tests after creating the ConvertToAudio function.

#### 4. Check if Audio Files Have Been Created in S3

**Step 1.** Navigate to the AWS S3 service page.

**Step 2.** Find and select the bucket `audioposts-19012003` (the name of the bucket you created).

**Step 3.** In the **Objects** tab, you will see the created audio files. You can select a file, download it, and listen to Polly's voice saying “This is working!”

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.041.png)
