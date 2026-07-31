---
title : "Create S3 Bucket"
date : 2026-07-31
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

In this section, we will create an Amazon S3 bucket to store user-uploaded files and configure CORS for the bucket.

![Create S3](/images/5-Workshop/5.4-Service/Create_S3.png)

#### Create S3 Bucket
1. In the search bar, search for **S3** and select it to navigate to the **S3** console.
2. Click on **Create bucket** to create a new bucket.
+ In the Bucket namespace section, select **Account Regional Namespace**.
+ Bucket name: `edushare-storage-2026`. Since we selected the Regional namespace above, the actual bucket name will have a string of characters appended to your base name.
+ Leave all other default settings unchanged and click **Create bucket** (or **Save**) at the bottom of the page.

![Create S3](/images/5-Workshop/5.4-Service/Create_S3_2.png)

#### Configure CORS for S3
This step is critical because users will upload files directly from their web browsers to S3 (via Presigned URLs). If CORS is not configured correctly, the browser will block these Cross-Origin requests.

1. Click on the name of the newly created bucket and switch to the **Permissions** tab.
2. Scroll down to the bottom of the page to find the **Cross-origin resource sharing (CORS)** section.
+ Click **Edit**.
+ Configure the CORS policy in JSON format to allow the necessary HTTP methods from all origins:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST",
            "DELETE",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "ETag"
        ],
        "MaxAgeSeconds": 3000
    }
]