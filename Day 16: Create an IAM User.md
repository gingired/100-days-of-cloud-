# Day 16: Create IAM User

## Overview

In this lab, I created a new AWS Identity and Access Management (IAM) user named **`user_67`** to establish fine-grained identity access control and manage permissions separate from the powerful root account.

---

## Lab Requirements

* **IAM Username:** `user_67`
* **Region:** Global / `us-east-1` (IAM service is global)
* **Action:** Create an IAM user and verify user summary creation

---

## Step-by-Step Implementation

### Step 1: Navigate to IAM Users

* Opened the AWS Management Console, searched for and navigated to the **IAM** dashboard.
* Selected **Users** from the left navigation pane.

### Step 2: Initialize User Creation

* Clicked on **Create user**.

### Step 3: Configure User Details

* Entered the specified username: **`user_67`** in the user details field.
* Clicked **Next** to proceed through the configuration settings toward **Review and create**.

### Step 4: Finalize Creation

* Reviewed the configuration summary and clicked **Create user** to complete the provisioning process.

---

## Verification & Results

* Redirected to the **Users** summary page.
* Confirmed that **`user_67`** appeared successfully in the list of active IAM identities.

---

## Key Takeaways

* **Principle of Least Privilege:** IAM users allow you to grant specific, limited permissions to individual users or applications rather than sharing powerful root credentials.
* **Global Scope:** IAM is a global AWS service, meaning users created are accessible across all regions within the AWS account.

<img width="1470" height="498" alt="image" src="https://github.com/user-attachments/assets/372a779e-6709-4a89-86b7-b3ce03a8fecd" />
