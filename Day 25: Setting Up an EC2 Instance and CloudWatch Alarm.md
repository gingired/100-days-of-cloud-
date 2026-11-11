# Day 25: Setting Up an EC2 Instance and CloudWatch Alarm

## Overview

In this lab, I provisioned an Ubuntu EC2 instance named **`devops-ec2`** and configured a CloudWatch alarm named **`devops-alarm`** to monitor CPU utilization, automatically triggering alerts via an existing SNS topic (**`devops-sns-topic`**) whenever usage exceeds defined operational thresholds.

---

## Lab Requirements

* **Instance Name:** `devops-ec2`
* **AMI Type:** Ubuntu AMI
* **Alarm Name:** `devops-alarm`
* **Metric:** CPU Utilization (Average)
* **Threshold:** $\ge 90\%$ for 1 consecutive 5-minute period
* **Alarm Action:** Send notification to `devops-sns-topic`
* **Region:** `us-east-1`

---

## Step-by-Step Implementation

### Step 1: Launch the EC2 Instance (`devops-ec2`)

* Opened the **EC2 Console** and clicked **Launch instance**.
* Configured the instance name as **`devops-ec2`**.
* Selected an appropriate **Ubuntu AMI** and a Free Tier eligible instance type (`t2.micro` or `t3.micro`).
* Attached a key pair, reviewed the summary settings, and clicked **Launch instance**.
* *Verification:* Confirmed the instance state transitioned to `Running` in the EC2 dashboard.

### Step 2: Navigate to CloudWatch Alarms via EC2 Console

* From the **EC2 Console $\rightarrow$ Instances** view, selected the newly launched **`devops-ec2`** instance.
* Clicked **Actions** $\rightarrow$ **Monitor and troubleshoot** $\rightarrow$ **Manage CloudWatch alarms**.
* Clicked **Create an alarm** under the **Add or edit alarm** section.

### Step 3: Configure CloudWatch Alarm Settings (`devops-alarm`)

* **Alarm Details:** Named the alarm **`devops-alarm`**.
* **Metric Configuration:**
* Left sample group as **Average** and metric type as **CPU Utilization**.


* **Alarm Thresholds:**
* Set condition: **`>=`** `90` percent.
* Set evaluation period: **1 consecutive period** of **5 minutes**.


* **Notification Actions:**
* Configured Amazon SNS notification actions.
* Selected the existing pre-created SNS topic: **`devops-sns-topic`**.


* Clicked **Create** to finalize the alarm creation.

---

## Verification & Results

* Verified that **`devops-alarm`** appears active under the CloudWatch alarms dashboard with proper metric bindings to **`devops-ec2`**.
* Confirmed that the alarm state transitions correctly link to **`devops-sns-topic`** for automated incident notifications.

---

## Key Takeaways

* **Proactive Infrastructure Monitoring:** Linking automated CloudWatch metrics and thresholds directly to SNS topics ensures rapid notification and incident response before performance degradation impacts end users.
* **Console-Driven Observability:** Managing alarms directly from the EC2 instance monitoring menu streamlines resource linking without manually searching through independent metric namespaces.

<img width="1454" height="695" alt="image" src="https://github.com/user-attachments/assets/c7ec971a-fafd-4ce9-8bb4-4778bf8b0fdb" />
<img width="1211" height="386" alt="image" src="https://github.com/user-attachments/assets/11f6e351-714f-4b31-add6-2d1f26894ded" />
