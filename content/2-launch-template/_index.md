+++
title = "Setting Up the Environment"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

## Initializing a CloudFormation Stack

We will use an AWS CloudFormation template to set up the resources needed for the workshop in the AWS Region of your choice. This is crucial as the subsequent instructions will depend on these resources. This CloudFormation template will provide the following resources:

- IAM Role
- Amazon DynamoDB Table
- AWS Step Functions State Machine

The steps are as follows:

**Step 1.** Download the CloudFormation template (YAML file) and save it in a separate folder on your computer: [Download here](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/template/workshop-stack.yaml).

**Step 2.** Open the [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).

**Step 3.** Choose **Create stack**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.002.png)

**Step 4.** Create the stack.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.003.png)

- Under **Prepare template**, choose **Choose an existing template**.
- Under **Template source**, select **Upload a template file**.
- Choose the template file you just downloaded and then select **Next**.

**Step 5.** Name your stack.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.004.png)

Here, I named it **TTS-serverless**. Select **Next**.

**Step 6.** Leave the stack configurations at default.

In the **Capabilities** section, allow AWS CloudFormation to create IAM resources.

Select **Submit** to deploy the template.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.005.png)
