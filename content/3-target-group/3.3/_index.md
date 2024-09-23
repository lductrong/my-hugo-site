+++
title = "Creating an SNS Topic"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++

In this task, we will create an SNS topic to facilitate communication between Lambda functions. Setting up the SNS topic is essential because the process of converting a post (in text format) to an audio file is divided into two AWS Lambda functions. This is due to several reasons:

1. **Asynchronous operation and quick response for users:**

   When a new post is submitted, we will immediately receive the post ID in DynamoDB even if the conversion to an audio file is not yet complete. This enhances the user experience, especially for larger posts, as users do not have to wait long.

2. **Resource optimization and scalability:**

   Separating the two processes allows for optimized resource usage and scaling of the system when needed. For example, the first Lambda function only needs to perform lightweight operations such as logging the post and saving it to DynamoDB, while the second function will handle the more complex conversion with Amazon Polly.

3. **Flexibility:**

   By separating the process of creating posts and converting audio, we create a more flexible and maintainable system.

## Steps to Follow:

**Step 1.** Navigate to the AWS Management Console and search for SNS.

**Step 2.** On the Amazon Simple Notification Service page, select **Topics**, then choose **Create topic**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.013.png)

**Step 3.** Configure the topic.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.014.png)

- **Type:** Select **Standard**.
- **Name:** Enter **new_posts**.
- **Display name:** Enter **New Posts**.

**Step 4.** At the bottom of the page, select **Create topic**.

**Step 5.** Save the ARN of the topic.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.015.png)

After creating, you will see the ARN of the topic (Amazon Resource Name).
{{% notice note %}}
Please note down this ARN for use in configuring the Lambda functions.
{{% /notice %}}
