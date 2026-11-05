# Day 19: Attach IAM Policy to IAM User

## Overview

In this lab, I attached the custom IAM policy **`iampolicy_gingired`** to the IAM user **`user_67`** to enforce specific access boundaries and permissions directly on the user profile.

---

## Lab Requirements

* **Target IAM User:** `user_67`
* **Target IAM Policy:** `iampolicy_gingired`
* **Region:** Global / `us-east-1` (IAM service is global)
* **Action:** Select an existing policy, navigate to policy actions, and attach it directly to the specified IAM user

---

## Step-by-Step Implementation

### Step 1: Navigate to IAM Policies

* Opened the AWS Management Console and navigated to the **IAM** dashboard.
* Selected **Policies** from the left navigation pane.

### Step 2: Select Policy and Trigger Attachment

* Located and selected **`iampolicy_gingired`** from the policy list.
* Clicked on the **Policy actions** dropdown menu and chose **Attach**.

### Step 3: Select Target User and Attach

* In the **Attach policy** configuration view, searched for and selected the target user **`user_67`**.
* Clicked **Attach policy** to finalize the association.

---

## Verification & Results

* Redirected back to the policy summary view.
* Verified under the **Policy usage** or user's **Permissions** tab that **`iampolicy_gingired`** was successfully linked to **`user_67`**.

---

## Key Takeaways

* **Direct User Permissions:** Attaching policies directly to an IAM user allows for fine-grained exception handling or standalone permissions outside of group memberships.
* **Auditing Access:** Regularly reviewing attached policies ensures that users only retain the permissions necessary for their current tasks, following security best practices.
<img width="1196" height="731" alt="image" src="https://github.com/user-attachments/assets/4479caec-230d-43e7-aa5c-541b6a7f2c3e" />
