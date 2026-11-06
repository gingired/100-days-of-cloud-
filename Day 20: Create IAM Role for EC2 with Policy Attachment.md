# Day 20: Create IAM Role for EC2 with Policy Attachment

## Overview

In this lab, I created an Amazon EC2 service IAM role named **`iamrole_gingired`** and attached a custom permission policy (**`iampolicy_gingired`**) to allow secure, role-based resource management without embedding long-term credentials.

---

## Lab Requirements

* **IAM Role Name:** `iamrole_gingired`
* **Trusted Entity Type:** `AWS service`
* **Use Case:** `EC2`
* **Attached Policy:** `iampolicy_gingired`
* **Region:** Global / `us-east-1` (IAM service is global)

---

## Step-by-Step Implementation

### Step 1: Navigate to IAM Roles

* Opened the AWS Management Console and navigated to the **IAM** dashboard.
* Selected **Roles** from the left navigation pane.

### Step 2: Select Trusted Entity Type

* Clicked on **Create role**.
* Selected **AWS service** under **Trusted entity type**.

### Step 3: Configure Use Case & Permissions

* Under **Service or use case**, selected **EC2** to establish the service trust relationship, then clicked **Next**.
* On the **Add permissions** step, searched for and selected the custom policy **`iampolicy_gingired`**, then proceeded.

### Step 4: Name and Create the Role

* On the final review screen, entered **`iamrole_gingired`** in the **Role name** field.
* Reviewed the trusted entities and permission policies summary, then clicked **Create role**.

---

## Verification & Results

* Redirected to the **Roles** dashboard.
* Confirmed that **`iamrole_gingired`** appeared in the active list and verified that **`iampolicy_gingired`** is successfully listed under its attached permissions tab.

---

## Key Takeaways

* **Secure Service Access:** IAM roles allow AWS services like EC2 instances to securely make API requests to other AWS resources without storing static access keys on the server.
* **Trust Relationships:** Defining **EC2** as the trusted entity automatically writes a trust policy permitting the EC2 service principal (`ec2.amazonaws.com`) to assume the role.
<img width="1183" height="463" alt="image" src="https://github.com/user-attachments/assets/51143ea0-5dd8-401d-a40f-d9fc14d9a172" />
