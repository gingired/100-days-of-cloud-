# Day 14: Terminate EC2 Instance

## Overview

In this lab, I terminated an obsolete Amazon EC2 instance (`devops-ec2` / `xfusion-ec2`) in the `us-east-1` region to clean up unused infrastructure resources following a migration process.

---

## Lab Requirements

* **Target Instance:** `devops-ec2` (`i-08f385e2ac174c3d3`)
* **Region:** `us-east-1` (N. Virginia)
* **Target State:** `terminated`
* **Action:** Remove termination and stop protections, then permanently delete the EC2 instance

---

## Step-by-Step Implementation

### Step 1: Navigate to EC2 Instances

* Opened the Amazon EC2 Console and navigated to **Instances $\rightarrow$ Instances**.
* Located and selected the target instance **`devops-ec2`** (`i-08f385e2ac174c3d3`).

### Step 2: Disable Termination Protection (`disableApiTermination`)

* Selected **Actions $\rightarrow$ Instance settings $\rightarrow$ Change termination protection**.
* Disabled the setting and saved changes to allow API and console deletion calls.

### Step 3: Disable Stop Protection (`disableApiStop`)

* Selected **Actions $\rightarrow$ Instance settings $\rightarrow$ Change stop protection**.
* Disabled the setting and saved changes to allow the instance to be stopped prior to termination.

### Step 4: Terminate Instance

* Clicked **Instance state $\rightarrow$ Terminate instance**.
* Confirmed termination in the modal dialog prompt.

---

## Challenges & Troubleshooting

### 1. API Termination Protection Block (`disableApiTermination`)

* **Struggle:** The initial termination attempt failed with a red console alert:
> *"Failed to terminate (delete) an instance: The instance 'i-08f385e2ac174c3d3' may not be terminated. Modify its 'disableApiTermination' instance attribute and try again."*
> 
> 


* **Root Cause:** The instance was launched with Termination Protection enabled, which explicitly guards virtual servers against accidental deletion via the console or CLI commands.
* **Resolution:** Navigated to **Actions $\rightarrow$ Instance settings $\rightarrow$ Change termination protection** and set the attribute to **Disabled**.

### 2. API Stop Protection Block (`disableApiStop`)

* **Struggle:** After disabling termination protection, a secondary error banner blocked the action:
> *"Failed to terminate (delete) an instance: The instance 'i-08f385e2ac174c3d3' may not be terminated. Modify its 'disableApiStop' instance attribute and try again."*
> 
> 


* **Root Cause:** Terminating an EC2 instance implicitly initiates a system stop request. Because Stop Protection (`disableApiStop`) was also enabled on the instance, AWS prevented state transitions to stopped/terminated.
* **Resolution:** Navigated to **Actions $\rightarrow$ Instance settings $\rightarrow$ Change stop protection** and set the attribute to **Disabled**.

---

## Verification & Results

* Successfully initiated deletion after removing both protection flags.
* Confirmed the instance state transitioned from `Running` $\rightarrow$ `shutting-down` $\rightarrow$ **`terminated`**.

---

## Key Takeaways

* **Defense-in-Depth for Critical Workloads:** `disableApiTermination` and `disableApiStop` protect production workloads from accidental or malicious deletion or shutdown commands.
* **Order of Operations:** Both termination and stop protection attributes must be toggled off before an instance with dual protections can be permanently removed from an AWS account.

![Terminate Instance Description](https://github.com/gingired/100-days-of-cloud-/raw/main/Terminate%20Instance.png)

