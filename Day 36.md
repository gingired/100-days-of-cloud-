# Day 36: Load Balancing EC2 Instances with Application Load Balancer

## Overview

In this lab, I provisioned an Ubuntu EC2 instance (`datacenter-ec2`) running an automated Nginx web server via user data and placed it behind an internet-facing **Application Load Balancer** (`datacenter-alb`) backed by a target group (`datacenter-tg`). By configuring security groups (`datacenter-sg` and `default`) to allow HTTP traffic on port `80` from clients to the ALB and from the ALB to the EC2 instance, I enabled high-availability traffic routing to the Nginx welcome page via the ALB DNS name.

---

## Lab Requirements

* **EC2 Instance Name:** `datacenter-ec2` (Ubuntu AMI, `Running` state, Nginx installed and started via user data)
* **EC2 Security Group:** `datacenter-sg` (allowing port `80` from the `default` security group attached to the ALB)
* **ALB Security Group:** `default` (allowing inbound `HTTP (80)` from `0.0.0.0/0`)
* **Target Group:** `datacenter-tg` (Instance target type, routing port `80` to `datacenter-ec2`)
* **Application Load Balancer:** `datacenter-alb` (Internet-facing, IPv4, spanning at least two subnets including the subnet hosting `datacenter-ec2`, routing port `80` to `datacenter-tg`)

---

## Step-by-Step Implementation

### Step 1: Create and Configure Security Groups (`datacenter-sg` and `default`)

* Opened the **EC2 Console** and selected **Security Groups** from the navigation pane.
* Created a new security group named **`datacenter-sg`** for `datacenter-ec2` and added an inbound rule allowing port `80` from the `default` security group.
* Updated the **`default`** security group's inbound rules to allow **HTTP (Port 80)** from `0.0.0.0/0` and saved the changes.

### Step 2: Launch the EC2 Instance (`datacenter-ec2`) with Nginx User Data

* Navigated to the **EC2 Dashboard** and clicked **Launch Instance**.
* Named the instance **`datacenter-ec2`** and selected an **Ubuntu** AMI.
* Under **Network settings**, chose an existing security group and selected **`datacenter-sg`**.
* Under **Advanced details**, added the following **User data** script to install, start, and enable Nginx on boot:
```bash
#!/bin/bash
apt update -y
apt install -y nginx
systemctl start nginx
systemctl enable nginx

```


* Clicked **Launch instance** and waited until `datacenter-ec2` reached the **`Running`** state.

### Step 3: Create the Target Group (`datacenter-tg`)

* Selected **Target Groups** from the EC2 navigation pane.
* Chose **Instance** as the target type, entered **`datacenter-tg`** as the target group name, and defined port `80`.
* Selected **`datacenter-ec2`**, clicked **Include as pending below**, verified it appeared under targets, and created the target group.

### Step 4: Create the Application Load Balancer (`datacenter-alb`)

* Navigated to **Load Balancers** (via the EC2 console), clicked **Create Load Balancer**, and selected **Application Load Balancer**.
* Configured basic settings with name **`datacenter-alb`**, **Internet-facing** scheme, and **IPv4** address type.
* Selected the VPC and at least two subnets (ensuring the subnet hosting `datacenter-ec2` is included), attached the **`default`** security group, and configured the port `80` listener to forward traffic to **`datacenter-tg`**.
* Clicked **Create load balancer**.

---

## Verification & Results

* Confirmed the load balancer reached the **`Active`** state in the AWS console.
* Opened the ALB DNS name in a browser and verified that the default **Nginx** welcome page loaded successfully.

---

## Key Takeaways

* **Layered Security Group Chaining:** Exposing an application through an ALB works cleanly when the ALB security group accepts external port `80` traffic and the instance security group (`datacenter-sg`) restricts port `80` to the ALB's security group rather than opening the instance directly to the internet.
* **Automated Instance Bootstrapping:** Using EC2 user data with `systemctl enable nginx` ensures web packages are installed and running automatically without manual post-launch SSH setup.
