+++
title = "Create a Serverless User Interface"
date = 2021
weight = 9
chapter = false
pre = "<b>3.9 </b>"
+++

Although the application is operational, it is currently only available as a RESTful web service. To facilitate easier user interaction, we will deploy a small website hosted on Amazon S3. Amazon S3 is an ideal choice for hosting static websites because it is simple, cost-effective, and easy to manage.

This website will use JavaScript to connect directly with the API we have created, providing text-to-speech functionality through the web interface. This allows users to enter text directly on the website and hear the generated speech results without directly interacting with the REST API.

## Steps to Implement:

Step 1. Download the following files to your machine. You can right-click and select “Save Link As…” or open the file and right-click to select “Save As…”

- [index.html](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/index.html)

- [scripts.js](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/scripts.js)

- [styles.css](https://static.us-east-1.prod.workshops.aws/public/2b2654d0-25fc-498c-9d95-069507fc0346/static/scripts/styles.css)

{{% notice note %}}
Keep the names and extensions of the files unchanged for proper functionality!
{{% /notice %}}

Step 2. Open the `scripts.js` file in any text editor (e.g., Notepad). At the first line, you will see:

```
var API\_ENDPOINT = "YOUR\_API\_GATEWAY\_ENDPOINT"
```


Replace “YOUR_API_GATEWAY_ENDPOINT” with the Invoke URL of the API you just deployed above. Remember to save the file afterward.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.064.png)

Step 3. Create an S3 bucket to store the 3 files.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.065.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.066.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.067.png)

- Navigate to the S3 service page.

- Select Create bucket and configure the following details:

- Bucket name: Since the name needs to be unique, we will name it: www-BUCKET, with BUCKET replaced by the name of the audioposts bucket you created earlier (e.g., my bucket name is: www-audioposts-19012003).

{{% notice note %}}
Remember to note down the bucket name for future use.
{{% /notice %}}

- For Object Ownership, select ACLs enabled.

- In the Block Public Access settings for this bucket: uncheck Block all public access and ensure that all other options are also unchecked.

- A warning box will appear; check the box “I acknowledge that the current settings might result in this bucket and the objects within becoming public.”

- Select Create bucket.

Step 4. Upload the 3 files to the Amazon S3 bucket.

Once the bucket has been created, select it from the bucket list.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.068.png)

Select Upload.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.069.png)

Select Add files and upload the 3 files: index.html, scripts.js, and styles.css. Click Upload to complete the file upload.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.070.png)

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.071.png)

{{% notice note %}}
The files must retain their original names: index.html, scripts.js, and styles.css.
{{% /notice %}}

Step 5. Return to the bucket page containing the 3 files, switch to the Permissions tab.

In the Bucket Policy section, select the Edit button.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.072.png)

Paste this Policy into the editor:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::www-BUCKET/*"
            ]
        }
    ]
}
```

{{% notice note %}}
Replace www-BUCKET with the name of your bucket.
{{% /notice %}}

Click Save changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.073.png)

Step 6. Finally, we will enable static website hosting, so the bucket will function as a static website.

- Switch to the Properties tab.

- Find the Static website hosting section and select Edit.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.074.png)

- Select Enable for Static website hosting.

- For Index document: enter index.html.

- For Error document - optional: enter index.html.

{{% notice note %}}
We are using the index.html file as the error document.
{{% /notice %}}

Click Save changes.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.075.png)

After saving, go back to the Static website hosting section and copy the Bucket website endpoint.

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.076.png)

And that’s it! You can check whether the website is operational.

Open a web browser tab and paste the Endpoint URL you just copied.

The website will open:

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.077.png)

If you enter something in the text box and select Say it, a new post will be sent to your application. The application will convert the text into an audio file.

To view the posts and their audio files, enter the post ID or \* (to display all) in the search box:

![](/images/Aspose.Words.e13c2680-26b7-4f33-be2e-ef4ed39807a7.078.png)

Depending on the size of the text you provide, the conversion process to audio may take some time. Posts that have not yet been converted will have a Status of PROCESSING.

Click Play to listen to the audio.

Website link: http://www-audioposts-19012003.s3-website-us-east-1.amazonaws.com/
