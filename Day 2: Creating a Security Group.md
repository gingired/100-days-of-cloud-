Day 2: Creating an AWS Security Group

Overview

In this lab, I created a custom security group named `devops-sg` under the default VPC on Amazon Web Services (AWS) using the EC2 Management Console. This security group acts as a virtual firewall for cloud resources, regulating incoming (inbound) and outgoing (outbound) traffic.

Requirements

- VPC: Default VPC
- Security Group Name: `devops-sg`
- Description: `Security group for Nautilus App Servers`

Inbound Rules:
- HTTP: Port 80, Source CIDR `0.0.0.0/0`
- SSH: Port 22, Source CIDR `0.0.0.0/0`

Step-by-Step Implementation

Step 1: Navigate to EC2 Dashboard

Logged in to the AWS Management Console and opened the EC2 Dashboard.

Step 2: Access Security Groups

Scrolled down to Network & Security in the left sidebar and clicked on Security Groups.

Step 3: Configure Security Group Details

Clicked on Create security group and filled in the following details:

- Security group name: `devops-sg`
- Description: `Security group for Nautilus App Servers`
- VPC: Default VPC

Step 4: Add Inbound Rules

Added the following inbound rules:

Rule 1:
- Type: HTTP
- Port range: 80
- Source: Anywhere (`0.0.0.0/0`)

Rule 2:
- Type: SSH
- Port range: 22
- Source: Anywhere (`0.0.0.0/0`)

Step 5: Review and Create

Clicked on Create security group to finalize the configuration.

Verification and Results

A green success notification appeared: "Security group (sg-096d1347ceae68281 | devops-sg) was created successfully".

Confirmed the following configuration details:

- Security Group Name: `devops-sg`
- Security Group ID: `sg-096d1347ceae68281`
- Inbound Rules Count: 2 permission entries

![Security Group Creation Confirmation](![AWS Security Group Creation](https://raw.githubusercontent.com/gingired/100-days-of-cloud-/main/Security-Group.png)


Key Takeaways

- Security Groups are stateful: Return traffic for permitted connections is automatically allowed, regardless of outbound rules.
- Traffic Control: Opening port 80 permits web traffic, while port 22 enables SSH access to associated instances.
- Security Best Practice: Restrict SSH access to trusted IP addresses instead of allowing access from anywhere (`0.0.0.0/0`) whenever possible.
