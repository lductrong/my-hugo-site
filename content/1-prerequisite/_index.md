+++
title = "Workshop Outline"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

## <a name="_v9ur3gv0eg5w"></a>Objectives
## <a name="_b98hs9rl3p9h"></a>After completing the workshop, you will be able to:
- Create an Amazon DynamoDB table to store data
- Create a RESTful API with Amazon API Gateway
- Create AWS Lambda functions triggered by API Gateway
- Connect AWS Lambda functions with Amazon Simple Notification Service (SNS)
- Use Amazon Polly to synthesize speech in various languages and tones

## <a name="_226ej8qlivw4"></a>Workshop Environment
Since we are building a serverless application, you do not need to work with servers—no need to provision resources, patch, or scale. AWS Cloud will handle all of this automatically, allowing you to focus on your application.

This application provides two main methods—one for sending information about a new post (which will be converted to an MP3 file) and one for retrieving information about an existing post (including the link to the MP3 file stored in an Amazon S3 bucket). Both of these methods are implemented as RESTful web services through **Amazon API Gateway**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.001.png)

#### <a name="_b8aslofboauv"></a>**When sending information about a new post:**
1. **Information** is sent via the RESTful web service provided by Amazon API Gateway. This service is called from a static website hosted on **Amazon S3**.
2. **Amazon API Gateway** triggers an **AWS Lambda function** named "New Post," which is responsible for initiating the MP3 file creation process.
3. This **Lambda function** inserts the post information into an **Amazon DynamoDB** table, where information about all posts is stored.
4. To handle the entire process asynchronously, the application uses **Amazon Simple Notification Service (SNS)** to decouple the process of receiving new post information from the process of converting the post to audio.
5. Another **Lambda function** named "Convert to Audio" is subscribed to this SNS topic and will be triggered whenever there is a new notification (indicating a new post needs to be converted to an audio file).
6. The **"Convert to Audio" Lambda function** uses **Amazon Polly** to convert the text into an audio file in the specified language (which must match the language of the text).
7. The new **MP3 file** is stored in an **S3 bucket**.
8. The post information is updated in the **DynamoDB table**, including the **URL of the audio file** stored in S3.

#### <a name="_ckkjfkkadct9"></a>**When retrieving information about a post:**
1. The RESTful web service implemented through **Amazon API Gateway** provides a method to retrieve post information. This method contains the text of the post and the link to the **S3 bucket** where the MP3 file is stored. This web service is also called from a static website hosted on **Amazon S3**.
2. **Amazon API Gateway** calls the **Get Post Lambda**, which implements the logic to retrieve the post data.
3. The **Get Post Lambda function** fetches the post information (including the link to **Amazon S3**) from the DynamoDB table and returns that information.
