# Day 37: Managing EC2 Access with S3 Role-based Permissions

## Overview

In this lab, I configured password-less SSH access from `aws-client` to an existing EC2 instance (`devops-ec2`), created a private S3 bucket (`devops-s3-18325`), and provisioned a scoped IAM policy and role (`devops-role`) granting `s3:PutObject`, `s3:ListBucket`, and `s3:GetObject` permissions. After attaching the role to `devops-ec2`, I verified S3 object upload and listing capabilities directly from the instance without using static access keys.

---

## Lab Requirements

* **EC2 Instance:** `devops-ec2` (existing instance requiring S3 bucket access)
* **SSH Configuration:** Key pair (`id_rsa` and `id_rsa.pub`) on `aws-client` with public key added to `/root/.ssh/authorized_keys` on `devops-ec2`
* **Private S3 Bucket:** `devops-s3-18325` (ACL disabled, Block all public access enabled)
* **IAM Policy & Role:** Policy allowing `s3:PutObject`, `s3:ListBucket`, and `s3:GetObject` on `devops-s3-18325`, attached to IAM role `devops-role` and assigned to `devops-ec2`

---

## Step-by-Step Implementation

### Step 1: Set Up SSH Keys and Access to `devops-ec2`

* Generated a new RSA SSH key pair on `aws-client`:
```bash
ssh-keygen -t rsa

```


* Added an inbound **SSH** rule allowing `0.0.0.0/0` in the `default` security group.
* Connected to `devops-ec2` via **EC2 Instance Connect** in the AWS Management Console.
* Switched to the `root` user and added the `aws-client` public key (`id_rsa.pub`) to `.ssh/authorized_keys`, then verified SSH login from `aws-client`:
```bash
sudo su -

```


```bash
ssh root@<devops-ec2-public-ip>

```



### Step 2: Create a Private S3 Bucket (`devops-s3-18325`)

* Opened the **Amazon S3** dashboard and clicked **Create bucket**.
* Entered **`devops-s3-18325`** under **General configuration**.
* Selected **ACL disabled** under **Object ownership** and ensured **Block all public access** remained checked so the bucket stays private.
* Left remaining options as default, clicked **Create bucket**, and confirmed the bucket appeared under **Buckets**.

### Step 3: Create an IAM Policy and Role (`devops-role`)

* Navigated to the **IAM Console** and selected **Policies** from the navigation pane.
* In the **Policy editor**, switched to **JSON** and defined permissions for `s3:ListBucket` on the bucket ARN and `s3:PutObject` / `s3:GetObject` on the object ARN pattern for `devops-s3-18325`:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowListBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::devops-s3-18325"
    },
    {
      "Sid": "AllowObjectOperations",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::devops-s3-18325/*"
    }
  ]
}

```


* Named and created the policy on the **Review and create** page.
* Selected **Roles** in the IAM navigation pane, chose **AWS service** as the trusted entity type, selected **EC2** under **Use case**, attached the S3 policy created above, and named the role **`devops-role`** before clicking **Create role**.

### Step 4: Attach the IAM Role to `devops-ec2`

* In the **EC2 Console**, selected `devops-ec2` and navigated to **Actions > Security > Modify IAM role**.
* Selected **`devops-role`** and saved the association.

---

## Verification & Results

* SSH'd into `devops-ec2`, created a test file (`secret.txt`), and uploaded it to `s3://devops-s3-18325/`:
```bash
touch secret.txt
aws s3 cp secret.txt s3://devops-s3-18325/

```


* Listed the contents of `s3://devops-s3-18325/` and verified `secret.txt` appeared in the bucket:
```bash
aws s3 ls s3://devops-s3-18325/

```



---

## Key Takeaways

* **Role-Based Temporary Credentials:** Attaching an IAM instance profile (`devops-role`) to EC2 provides temporary, automatically rotated credentials to the AWS CLI/SDK without storing long-lived access keys on disk.
* **Bucket vs. Object ARN Scope:** Bucket-level actions such as `s3:ListBucket` target the bucket ARN (`arn:aws:s3:::bucket-name`), whereas object-level actions like `s3:PutObject` and `s3:GetObject` target object ARNs (`arn:aws:s3:::bucket-name/*`).
