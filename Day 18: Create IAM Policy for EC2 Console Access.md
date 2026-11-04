# Day 18: Create Read-Only IAM Policy for EC2 Console Access

## Overview

In this lab, I created a custom AWS Identity and Access Management (IAM) policy named **`iampolicy_gingired`** to grant read-only access to the Amazon EC2 console (allowing users to view instances, AMIs, and snapshots without granting modification or administrative privileges).

---

## Lab Requirements

* **IAM Policy Name:** `iampolicy_gingired`
* **Region:** Global / `us-east-1` (IAM service is global)
* **Permissions:** Read-only access to Amazon EC2 resources (Instances, AMIs, Snapshots)
* **Action:** Create a custom IAM policy using the console, define resource scopes, and verify its definition

---

## Step-by-Step Implementation

### Step 1: Navigate to IAM Policies

* Opened the AWS Management Console and navigated to the **IAM** dashboard.
* Selected **Policies** from the left navigation pane.

### Step 2: Initialize Policy Creation

* Clicked on **Create policy**.

### Step 3: Configure Service, Access Levels, & Resources

* In the **Policy editor**, selected the **Visual** option.
* Chose **EC2** as the target service.
* Expanded the **Access level** groups and selected read-only permissions (such as **List** and **Read** access actions) to allow viewing instances, AMIs, and snapshots while blocking write operations.
* Under the **Resources** section, configured the scope to **All resources (`*`)** (or specified resource ARNs) to satisfy the resource requirement prompt.

### Step 4: Review and Name Policy

* Proceeded to the **Review and create** page.
* Entered **`iampolicy_gingired`** in the **Policy name** field.
* Added an optional description (e.g., *Read-only access policy for EC2 console viewing*).
* Clicked **Create policy** to save the custom policy.

---

## Verification & Results

* Redirected to the **Policies** dashboard.
* Confirmed that **`iampolicy_gingired`** appeared successfully in the list of customer-managed policies with the appropriate read-only access scopes.

---

## Key Takeaways

* **Resource Specification:** AWS IAM visual policies require explicit resource bindings for all selected actions; selecting **All resources (`*`)** or defining precise ARNs resolves missing resource configuration errors.
* **Least Privilege Principle:** Custom read-only policies ensure that users or teams can inspect infrastructure components (like running servers, snapshots, and AMIs) for auditing or troubleshooting without risking accidental modifications or deletions.
<img width="1156" height="659" alt="image" src="https://github.com/user-attachments/assets/aecd4d38-af60-449e-bf67-08ea47baae66" />
