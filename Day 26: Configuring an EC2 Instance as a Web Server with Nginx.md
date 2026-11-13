# Day 26: Configuring an EC2 Instance as a Web Server with Nginx

---

## 1. Objective & Specifications

* **Goal:** Create an AWS EC2 instance named `datacenter-ec2`, automate Nginx installation via user data, and allow web traffic over HTTP.
* **Instance Name:** `datacenter-ec2`
* **AMI:** Ubuntu (Latest LTS)
* **Instance Type:** `t3.micro`
* **Security Group Rules:**
* Allow SSH (Port 22) from `0.0.0.0/0`
* Allow HTTP (Port 80) from `0.0.0.0/0`



---

## 2. Step-by-Step Implementation Process

1. **Launched the EC2 Instance:**
1. Navigated to the **AWS Management Console** > **EC2 Dashboard** > **Instances** > **Launch instance**.
2. Assigned the instance name **`datacenter-ec2`**.
3. Selected the **Ubuntu** AMI and chose the **`t3.micro`** instance type.
4. Selected my existing SSH key pair (`.pem`) for secure remote access.
5. Configured the Security Group to explicitly allow inbound traffic on **Port 22 (SSH)** and **Port 80 (HTTP)**.
*Verification:* The instance successfully transitioned to the `Running` state with status checks passing (`3/3 checks passed`).




2. **Configured the User Data Script:**
1. Expanded the **Advanced details** section at the bottom of the launch configuration page.
2. Pasted the automated provisioning script into the **User data** field:

```bash
#!/bin/bash
apt update -y
apt install -y nginx
systemctl start nginx
systemctl enable nginx

```

3. Clicked **Launch instance** to initiate the server setup.
*Verification:* The automated script executed upon boot, installing and starting Nginx without manual intervention.


3. **Secured Key Permissions & Connected via SSH:**
1. Opened my local terminal and navigated to the directory containing my `.pem` private key.
2. Set the required strict read-only file permissions:

```bash
chmod 400 your-key-pair.pem

```

3. Logged into the instance using its Public IPv4 address:



```bash
ssh -i "your-key-pair.pem" ubuntu@<YOUR_INSTANCE_PUBLIC_IP>

```

*Verification:* The terminal prompt successfully updated to `ubuntu@ip-...`, confirming active SSH access to the cloud environment.


4. **Verified Nginx Service and Web Traffic:**
1. Checked the operational status of the web server daemon inside the instance:

```bash
sudo systemctl status nginx

```

2. Copied the instance's **Public IPv4 address**, pasted it into a web browser address bar, and loaded the page.
*Verification:* The service status output displayed **active (running)** in green text, and the web browser successfully rendered the default **"Welcome to nginx!"** landing page.
<img width="1469" height="238" alt="image" src="https://github.com/user-attachments/assets/73e6b963-1b73-441d-a927-bee85827328d" />
