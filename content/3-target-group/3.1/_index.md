+++
title = "Create DynamoDB Table"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++

In this task, we will create a table in DynamoDB to store information about posts, including the text content and the corresponding MP3 file URL. We will create a "posts" table with a primary key (`id`) that is a string generated by the "New Post" Lambda function when a new post is inserted into the database.

## The steps are as follows:

**Step 1.** Navigate to the DynamoDB service page.

**Step 2.** Select **Create table**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.006.png)

**Step 3.** Set up the table as follows:

- **Table name:** posts

- **Partition key:** id

- **Table settings:** Select the default **Default settings**.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.007.png)

**Step 4.** Click **Create table** to create the table.
