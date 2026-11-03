# Day 17: Create IAM Group

## Overview

In this lab, I created a new AWS Identity and Access Management (IAM) user group named **`iamgroup_67`** to manage and assign permissions collectively to multiple users rather than managing policies on an individual basis.

---

## Lab Requirements

* **IAM Group Name:** `iamgroup_67`
* **Region:** Global / `us-east-1` (IAM service is global)
* **Action:** Create an IAM group, add member users (such as `user_67`), attach permission policies, and verify creation

---

## Step-by-Step Implementation

### Step 1: Navigate to IAM User Groups

* Opened the AWS Management Console and navigated to the **IAM** dashboard.
* Selected **User groups** from the left navigation pane.

### Step 2: Initialize Group Creation

* Clicked on **Create group**.

### Step 3: Configure Group Details & Permissions

* Entered **`iamgroup_67`** in the **User group name** field.
* Selected member users (such as **`user_67`**) from the users list to include them in the group.
* Selected the desired permission policies to apply to all members of the group.

### Step 4: Finalize Group Creation

* Reviewed the configuration and clicked **Create group** to complete the provisioning process.

---

## Verification & Results

* Redirected to the **User groups** dashboard.
* Confirmed that **`iamgroup_67`** successfully appeared in the active list of user groups.

---

## Key Takeaways

* **Group-Based Permissions:** IAM groups make permission management scalable. Instead of attaching policies to individual users, permissions are assigned to the group, and users inherit them automatically upon joining.
* **Naming Conventions:** IAM group names can contain up to 128 characters (letters, numbers, and standard symbols like `+=,.@_-`), and must be unique within the account.

<img width="1168" height="338" alt="image" src="https://github.com/user-attachments/assets/ea9b7bb1-7782-4c08-bec6-a21e69567c0a" />
