# aws-s3-static-website
Multi-page static website hosted on Amazon S3 with custom bucket policies and error handling

# ☁️ AWS Project: Hosting a Multi-Page Static Website on Amazon S3

This repository contains my hands-on project for deploying a multi-page static website using Amazon S3. This project demonstrates cloud infrastructure fundamentals, serverless hosting, object storage, and AWS access governance.



## 🏗️ Architecture

Instead of relying on traditional web servers like Apache or Nginx on EC2 instances, this project uses Amazon S3 to serve static content directly:

```text
                 Internet User
                       |
                       v
             S3 Website Endpoint
                       |
                       v
               Amazon S3 Bucket
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
   index.html      about.html     labs.html
        |
        +----> services.html
        |
        +----> contact.html
        |
        +----> style.css
        |
        +----> error.html

```


## 🛠️ Step-by-Step Deployment Guide

### **Step 1: Prepare the Website Files**

Ensure you have the website source files ready on your local machine. The project structure should look like this:

```text
aws-s3-static-website/
├── index.html
├── about.html
├── services.html
├── labs.html
├── contact.html
├── error.html
└── style.css

```

### **Step 2: Create an S3 Bucket**

1. Open the AWS Console and search for **S3**.
2. Click **Create bucket**.
3. Enter a globally unique bucket name (e.g., `salahuddin-aws-static-website-12345`).
4. Select your preferred AWS Region.
5. Keep the remaining settings at their defaults and click **Create bucket**.

### **Step 3: Upload the Website Files**

1. Open your new S3 bucket.
2. Click **Upload** -> **Add files**.
3. Select all the HTML and CSS files and click **Upload**.
4. Verify that all files are in the **root** of the bucket (not inside a subfolder).

### **Step 4: Enable Versioning and Static Website Hosting**

1. Go to the bucket **Properties** tab.
2. Under **Bucket Versioning**, click Edit and select **Enable**.
3. Scroll down to **Static website hosting** and click **Edit**.
4. Select **Enable** and choose **Host a static website**.
5. Configure the documents:
* **Index document:** `index.html`
* **Error document:** `error.html`


6. Click **Save changes** and copy the generated **Bucket website endpoint**.

### **Step 5: Disable Block Public Access**

*Note: If you test the endpoint now, you will get a `403 Forbidden` error because the objects are not publicly readable.*

1. Open the **Permissions** tab.
2. Find **Block public access (bucket settings)** and click **Edit**.
3. Uncheck **Block all public access**.
4. Acknowledge the AWS warning and save the changes.

### **Step 6: Add the Website Bucket Policy**

1. Stay in the **Permissions** tab and scroll down to **Bucket policy**.
2. Click **Edit** and paste the following JSON policy (replace `YOUR-BUCKET-NAME` with your actual bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}

```

3. Click **Save changes**.

### **Step 7: Verify the Live Website**

1. Open the **Bucket website endpoint** in your browser.
2. **Test Navigation:** Click through the different pages to ensure relative routing works without 403 errors.
3. **Test CSS:** Verify the stylesheet (`style.css`) loads correctly across all pages.
4. **Test Custom Error Page:** Manually add `/not-found.html` to the end of your URL. Verify that it routes to the custom `error.html` page instead of the default AWS error.



## 🧹 Security & Clean-Up

To avoid unnecessary charges and maintain security after testing the infrastructure:

1. Open **Permissions** and remove the public bucket policy.
2. Re-enable **Block all public access**.
3. Open **Properties** and disable **Static website hosting**.
4. Delete the files from the bucket, then delete the bucket itself.


**Focus Area:** AWS Cloud Infrastructure, Object Storage, IAM Policies, and Serverless DevOps.
