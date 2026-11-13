# Day 27: Configuring a Public VPC with an EC2 Instance for Internet Access

## Overview

In this lab, I set up a custom public Virtual Private Cloud (VPC) named **`xfusion-pub-vpc`**, configured a public subnet named **`xgingi-pub-subnet`** with automatic public IP address assignment, and provisioned an EC2 instance named **`xgingi-pub-ec2`** (`t3.micro`) with open SSH access to support public-facing services.

---

## Lab Requirements

* **VPC Name:** `xfusion-pub-vpc`
* **Subnet Name:** `xgingi-pub-subnet` (with Auto-assign public IPv4 enabled)
* **Instance Name:** `xgingi-pub-ec2`
* **Instance Type:** `t3.micro`
* **Security Group Rule:** Port `22` (SSH) open to the internet (`0.0.0.0/0`)
* **Region:** `us-east-1`

---

## Step-by-Step Implementation

### Step 1: Create the Public VPC and Subnet Using "VPC and more"

* Opened the **VPC Console** and clicked **Create VPC**.
* Selected **VPC and more**, which automatically provisions associated networking components (Internet Gateway, route tables, and subnets).
* Configured resource naming and set the IPv4 CIDR block (`10.0.0.0/16`).
* Set the number of **Availability Zones (AZs)** to **1** and configured **1 public subnet**.
* Renamed the generated subnet to **`xgingi-pub-subnet`**.
* Ensured that **Auto-assign public IPv4 address** was enabled for the subnet so that resources automatically receive a public IP.
* Clicked **Create VPC**.

### Step 2: Launch the EC2 Instance (`xgingi-pub-ec2`)

* Navigated to the **EC2 Console $\rightarrow$ Launch instance**.
* Named the instance **`xgingi-pub-ec2`**.
* Selected an appropriate Linux AMI and set the instance type to **`t3.micro`**.
* Under **Network settings**, switched the **VPC** dropdown from `(default)` to the custom public VPC, and selected **`xgingi-pub-subnet`**.
* Verified that **Auto-assign public IP** was enabled.
* Configured a security group to allow **SSH (Port 22)** traffic from anywhere (`0.0.0.0/0`).
* Reviewed configurations and clicked **Launch instance**.

---

## Troubleshooting & Challenges Encountered

### 1. Subnet Dropdown Visibility In the EC2 Launch Wizard

* **Issue:** When attempting to launch the EC2 instance, the custom public subnet did not appear in the subnet selection dropdown list.
* **Resolution:** Identified that the VPC selector right above the subnet dropdown was defaulted to the `(default)` VPC instead of the newly created custom public VPC. Switching the VPC field to the custom VPC immediately exposed the corresponding `xgingi-pub-subnet`.

---

## Verification & Results

* Verified that **`xgingi-pub-ec2`** successfully resides inside the custom public VPC and correctly inherited a public IPv4 address from **`xgingi-pub-subnet`**.
* Confirmed that SSH connectivity over port `22` is fully operational for external management.

---

## Key Takeaways

* **Automated VPC Provisioning:** Utilizing the "VPC and more" option streamlines network topology creation by automatically wiring Internet Gateways and route tables.
* **VPC Scope Context:** EC2 launch wizards filter available subnets strictly based on the parent VPC selected in the network settings, making correct VPC selection a mandatory prerequisite.
<img width="1280" height="510" alt="image" src="https://github.com/user-attachments/assets/23d5e228-b759-45c7-87d6-4748467852c2" />
<img width="1280" height="510" alt="image" src="https://github.com/user-attachments/assets/af790193-6d53-4b1f-bfe5-b86d249997c4" />
<img width="1280" height="674" alt="image" src="https://github.com/user-attachments/assets/bd28f6dc-e7b8-40fc-b54f-93ffca19f074" />
