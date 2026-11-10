# Day 24: Setting Up an Application Load Balancer for an EC2 Instance

## Overview

In this lab, I set up an Application Load Balancer (ALB) named **`xfusion-gingired`** to route public web traffic from port `80` to an underlying EC2 instance hosting an Nginx server, utilizing a custom security group and target group configuration.

---

## Lab Requirements

* **Load Balancer Name:** `xfusion-gingired` (or `xfusion-alb`)
* **Target Group Name:** `xgingired-tg` (or `xfusion-tg`)
* **Security Group Name:** `xfusion-gingired` (allowing public inbound traffic on port `80`)
* **Region:** `us-east-1`
* **Objective:** Distribute incoming internet traffic across target EC2 instances running Nginx.

---

## Step-by-Step Implementation

### Step 1: Create the Security Group (`xfusion-gingired`)

* Navigated to the **EC2 Console $\rightarrow$ Security Groups $\rightarrow$ Create security group**.
* Named the security group **`xfusion-gingired`** and added a description.
* Configured inbound rules to open port `80` (HTTP) from the public internet (`0.0.0.0/0`).
* *Verification:* Checked that the security group was successfully generated with the correct VPC ID and inbound rules.

### Step 2: Create the Target Group (`xgingired-tg`)

* Navigated to **EC2 Console $\rightarrow$ Target Groups $\rightarrow$ Create target group**.
* Selected **Instances** as the target type and named the target group **`xgingired-tg`**.
* Set the protocol to **HTTP** on port `80` and selected the correct VPC (`vpc-0115528e110f551ed`).
* Registered the target EC2 instance, clicked **Include as pending below**, and finalized creation.
* *Verification:* Confirmed the target group status is active and linked to the proper VPC.

### Step 3: Create the Application Load Balancer (`xfusion-gingired`)

* Navigated to **EC2 Console $\rightarrow$ Load Balancers $\rightarrow$ Create Load Balancer** $\rightarrow$ **Application Load Balancer**.
* Configured the load balancer name as **`xfusion-gingired`**, set the scheme to **Internet-facing**, and selected **IPv4**.
* Under **Network mapping**, selected the VPC and enabled subnets across multiple Availability Zones (`us-east-1a`, `us-east-1b`, `us-east-1c`).
* Attached the newly created security group (`xfusion-gingired`) and routed listener traffic to the `xgingired-tg` target group.
* Clicked **Create load balancer**.
* *Verification:* Verified that the load balancer state changed from `Provisioning` to `Active` and successfully generated a public DNS name.

---

## Troubleshooting & Challenges Encountered

### 1. Security Group Port Routing

* **Issue:** Forgetting to allow inbound HTTP traffic on port `80` in the load balancer's security group would result in connection timeouts from public browsers.
* **Resolution:** Explicitly added an inbound rule permitting TCP port `80` from `0.0.0.0/0` during security group creation.

### 2. Target Health Checks

* **Issue:** Newly registered targets can momentarily show as unhealthy while the Nginx service initializes and the load balancer completes its initial health check probes.
* **Resolution:** Allowed a few minutes for target registration and verified that Nginx was actively running and responding on port `80` of the backend EC2 instance.
<img width="1230" height="618" alt="image" src="https://github.com/user-attachments/assets/398532ba-c817-49c7-b7a2-d0ca946f3ef2" />
<img width="1230" height="432" alt="image" src="https://github.com/user-attachments/assets/df290f62-97ff-431b-ad52-a398ba8d703a" />
<img width="1230" height="432" alt="image" src="https://github.com/user-attachments/assets/461fb5e5-0e8a-448a-8017-2be2d85e010c" />
<img width="1230" height="618" alt="image" src="https://github.com/user-attachments/assets/8996d310-2fa7-462a-85f1-05859087a007" />
