+++
title = "Expose Lambda Functions as RESTful Web Services"
date = 2021
weight = 8
chapter = false
pre = "<b>3.8 </b>"
+++

Now that our logic processing is set up, we will use Amazon API Gateway to expose our Lambda functions as RESTful web services. This allows us to call them easily using HTTP protocols.

## Create API Gateway

Step 1. In the AWS Management Console, search for API Gateway.

Step 2. Among the API types, we will use REST API and select Build.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.050.png)

Step 3. Create REST API.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.051.png)

Select “New API.”

API name: Name it: PostReaderAPI.

Description: API for TTS application.

API endpoint type: select Regional.

Choose Create API.

## Configure HTTP Methods

#### POST Method

Step 1. In the Resources panel, select the root resource (/).

Step 2. Select Create method.

Step 3. Configure the method.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.053.png)

- Method type: Select POST.

- Integration type: Choose Lambda Function.

- Select the function containing PostReader_NewPost.

- Choose Create method to complete the creation process.

#### GET Method

Step 1. In the Resources panel, select the root resource (/).

Step 2. Select Create method.

Step 3. Configure the method.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.054.png)

- Method type: Select GET.

- Integration type: Choose Lambda Function.

- Select the function containing PostReader_GetPost.

- Choose Create method to complete the creation process.

## <a name="_rphnw3flytm1"></a>Enable CORS

Cross-Origin Resource Sharing (CORS) allows API calls from different domains.

Step 1. In the Resources panel, select the root resource (/).

Step 2. Select Enable CORS.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.055.png)

Step 3. Configure settings.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.056.png)

Gateway responses: Select “Default 4XX” and “Default 5XX.”

Access-Control-Allow-Methods: Select GET and POST.

Click Save to finalize the configuration.

## <a name="_jpehrfs77t3o"></a>Configure Query Parameters

Next, we will configure the **GET** method to accept a **query parameter** named **postId**. This parameter will provide information about the **ID of the post** you want to retrieve.

Step 1. Select the GET method.

Step 2. In the Method request section, select Edit.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.057.png)

Step 3. Click on URL query string parameters to expand it.

Step 4. Select Add query string and parameter with Name: postId.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.058.png)

Step 5. Click Save.

## <a name="_5ud6upiakn7u"></a>Set Up Request Mapping

The **PostReader_GetPost Lambda** function requires the input data to be in **JSON** format, so we need to configure the API to convert the parameters sent by the user (such as **postId** from the query parameter) into JSON format before passing it to the Lambda function.

Step 1. Select the GET method.

Step 2. Go to the Integration Request tab and select Edit.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.059.png)

Step 3. Adjust the settings.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.060.png)

- For Request body passthrough: select “When there are no templates defined (recommended).”

- Click on Mapping Templates to expand it.

- Content-Type: Enter application/json.

- Template body: Enter:

```
{
    "postId" : "$input.params('postId')"
}
```
- Click Save to save the changes.

## <a name="_uhb4rxfrg3mn"></a>Deploy API

After all configurations, the API is ready to be deployed!

Step 1. Select Deploy API.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.061.png)

Step 2. 

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.062.png)

- Stage: Select *New Stage*.

- Stage name: Name it: Dev.

- The description section can be skipped; select Deploy.

Step 3. After a successful deployment, save the Invoke URL for future use.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.063.png)
