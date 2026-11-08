# Day 21: Setting Up an EC2 Instance with an Elastic IP for Application Hosting

## Overview

In this lab, I provisioned a new Amazon EC2 instance named **`datacenter-ec2`** of type `t2.micro` and associated a dedicated, static **Elastic IP** named **`datacenter-eip`** to ensure a reliable and consistent access point for application hosting.

---

## Lab Requirements

* **Instance Name:** `datacenter-ec2`
* **Instance Type:** `t2.micro`
* **Elastic IP Name:** `datacenter-eip`
* **Region:** `us-east-1` (N. Virginia)
* **Objective:** Launch a Linux instance and bind a persistent Elastic IP address to maintain a stable public IP across reboots.

---

## Step-by-Step Implementation

### Step 1: Launch the EC2 Instance (`datacenter-ec2`)

* Opened the **EC2 Console** and clicked on **Launch instance**.
* Configured the instance name as **`datacenter-ec2`**.
* Selected a Free Tier eligible Linux AMI (such as Amazon Linux or Ubuntu) and a **`t2.micro`** instance type.
* Selected or created a key pair for secure shell access, reviewed the summary panel, and clicked **Launch instance**.

### Step 2: Allocate an Elastic IP Address (`datacenter-eip`)

* Navigated to the **Amazon VPC Console** via the AWS Management Console search bar.
* Selected **Elastic IPs** from the left navigation pane.
* Clicked **Allocate Elastic IP address**, kept the default Amazon pool settings, and clicked **Allocate**.
* Tagged and renamed the allocated Elastic IP with the name tag **`datacenter-eip`** for resource identification.

### Step 3: Associate the Elastic IP with `datacenter-ec2`

* From the **Elastic IPs** list, selected the newly allocated **`datacenter-eip`**.
* Clicked **Actions** $\rightarrow$ **Associate Elastic IP address**.
* Under **Resource type**, selected **Instance**.
* Searched for and selected the target instance **`datacenter-ec2`** (or its specific instance ID), chose the primary private IP address, and clicked **Associate**.

---

## Verification & Results

* Verified that **`datacenter-ec2`** is running with its assigned resource tags.
* Confirmed inside the **Elastic IPs** console dashboard that **`datacenter-eip`** successfully displays an active association pointing to the instance's network interface and private/public IP address mapping.

---

## Key Takeaways

* **Static Public IP Management:** Standard EC2 public IP addresses change if an instance is stopped and started. Allocating and associating an Elastic IP provides a permanent, fixed public IP address crucial for production application endpoints, DNS records, and firewall whitelisting.
* **VPC Resource Binding:** Elastic IPs are tied to the AWS VPC networking layer rather than a single ephemeral server instance, allowing administrators to re-map the static IP to replacement instances seamlessly during disaster recovery or maintenance.
<img width="1469" height="260" alt="image" src="https://github.com/user-attachments/assets/b0763180-5776-4419-9b12-0eabd149b3ca" />
<img width="1469" height="352" alt="image" src="https://github.com/user-attachments/assets/c2029df0-0563-4003-8167-3d542580c703" />
