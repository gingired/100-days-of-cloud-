# Day 39: Hosting a Static Website on AWS S3

## Overview

In this lab, I created a public-facing Amazon S3 bucket (`nautilus-web-13898`), enabled static website hosting with `index.html` as the index document, configured bucket public access and a read-only bucket policy (`s3:GetObject`), and uploaded `index.html` from the AWS client host (`/root/`). Finally, I verified that the static portal rendered directly through the S3 website endpoint URL.

---

## Lab Requirements

* **S3 Bucket Name:** `nautilus-web-13898`
* **Static Website Hosting:** Enabled with `index.html` set as the index document
* **Public Access:** Block public access disabled for the bucket and public read access granted via bucket policy (`s3:GetObject`)
* **Source File:** `index.html` uploaded from `/root/` on the AWS client host
* **Verification:** Website accessible directly via the S3 **Bucket website endpoint** URL

---

## Step-by-Step Implementation

### Step 1: Create the S3 Bucket (`nautilus-web-13898`)

* Opened the **Amazon S3 Console** and clicked **Create bucket**.
* Entered **`nautilus-web-13898`** as the bucket name and selected the target AWS region.
* Under **Block Public Access settings for this bucket**, unchecked **Block all public access**, kept remaining options as default, and clicked **Create bucket**.

### Step 2: Enable Static Website Hosting and Configure Bucket Policy

* In the **Amazon S3 Console**, chose **General purpose buckets** in the left navigation pane and selected **`nautilus-web-13898`**.
* Opened the **Properties** tab, located **Static website hosting**, and clicked **Edit**.
* Selected **Enable**, entered **`index.html`** in **Index document**, and clicked **Save changes**.
* Under the **Permissions** tab, edited the **Bucket policy** to grant public read access (`s3:GetObject`) to objects in `nautilus-web-13898`:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::nautilus-web-13898/*"
    }
  ]
}

```



### Step 3: Upload `index.html` from the AWS Client Host

* Uploaded `index.html` from `/root/` on the AWS client host to `s3://nautilus-web-13898/` using the AWS CLI:
```bash
aws s3 cp index.html s3://nautilus-web-13898/

```


* Verified that `index.html` is listed inside the bucket objects.

---

## Verification & Results

* Opened the **Properties** tab for `nautilus-web-13898`, navigated to **Static website hosting**, and clicked the **Bucket website endpoint** link.
* Confirmed that the website loaded in the browser and displayed `Welcome to KKE labs!`.

---

## Key Takeaways

* **Two-Part Public Access for Static Sites:** Making an S3 static website publicly reachable requires both disabling **Block all public access** (at the bucket/account level) and applying a **Bucket policy** allowing `s3:GetObject` on `arn:aws:s3:::<bucket-name>/*`.
* **Website Endpoint vs. REST Endpoint:** Static index document routing (`index.html`) is served through the S3 **Bucket website endpoint** rather than the standard REST API endpoint.
