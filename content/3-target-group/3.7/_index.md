+++
title = "Create Lambda Function to Get Post"
date = 2021
weight = 7
chapter = false
pre = "<b>3.7 </b>"
+++

In this task, we will create a function to retrieve information about existing posts in DynamoDB.

## Steps to Create the Function:

   Step 1. Navigate to the Lambda service page.

   Step 2. Select Create function.

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.042.png)

   Step 3. Set up the function.

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.043.png)

   ![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.044.png)

   - Select the option Author from scratch.

   - Function name: Name the function: PostReader_GetPost.

   - Runtime: Choose Python 3.12.

   - Click on Change default execution role to expand the default execution role options.

        + Execution role: Select Use an existing role.

        + Existing role: Choose Lab-Lambda-Role.

   - Select create function to complete the function creation process.

   Step 4. Add code to the function.

   Delete all existing code in the function and insert the Python code:

``` python
import boto3
import os
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["postId"]

    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])

    if postId=="*":
        items = table.scan()
    else:
        items = table.query(
            KeyConditionExpression=Key('id').eq(postId)
        )

    return items["Items"]
```

   Step 5. Select Deploy to save changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.045.png)

Similar to the two functions we created earlier, we will now add the name of the DynamoDB table to the environment variable for the function.

Step 6. Configure Environment Variables

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.046.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.047.png)

Navigate to the "Configuration" tab.

In the left panel, select "Environment variables."

Click on "Edit" and add the following variable:

+ Key: Enter: DB_TABLE_NAME, Value: Enter: posts

Click on "Save."

Step 7. Test the Function

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.048.png)

Select the "Test" tab.

Choose Create new event.

Event name: Enter AllPosts.

Event JSON:

```
{
    "postId": "\*"
}
```

Click Save and then select Test.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.049.png)

A success message will be displayed. Click on Details to see the results of the test.
