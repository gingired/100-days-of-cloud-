# Day 30: Enable Internet Access for Private EC2 using NAT Instance

## Overview

In this lab, I configured and deployed a custom **NAT Instance** (`devops-nat-instance`) within a new public subnet (`devops-pub-subnet`) inside an existing private VPC (`devops-priv-vpc`). By configuring kernel IP forwarding, `iptables` masquerading, disabling source/destination checks, and updating the private subnet route table, I successfully enabled secure outbound internet access for `devops-priv-ec2` to upload test files to an S3 bucket (`devops-nat-18335`).

---

## Lab Requirements

* **Existing VPC:** `devops-priv-vpc`
* **Existing Private Subnet:** `devops-priv-subnet` (`10.1.1.0/24`)
* **Existing Private Instance:** `devops-priv-ec2`
* **Target S3 Bucket:** `devops-nat-18335`
* **New Public Subnet:** `devops-pub-subnet`
* **NAT Instance:** `devops-nat-instance` (Amazon Linux 2 AMI, deployed in `devops-pub-subnet` with Auto-assign public IP enabled)
* **Internet Gateway:** `igw-priv-vpc` attached to `devops-priv-vpc`

---

## Step-by-Step Implementation

### Step 1: Create the Public Subnet and Configure Internet Gateway

* Opened the **VPC Console** and chose **Create subnet**, selecting `devops-priv-vpc`.
* Named the subnet **`devops-pub-subnet`** and assigned a non-overlapping CIDR block.
* Created a new Internet Gateway named **`igw-priv-vpc`** and attached it to `devops-priv-vpc`.
* Created a new route table named **`public-route-table`** under `devops-priv-vpc`, which automatically associated with the Internet Gateway via a `0.0.0.0/0` route.
* Associated `devops-pub-subnet` with `public-route-table` to convert it into a fully functional public subnet.

### Step 2: Launch and Configure the NAT Instance (`devops-nat-instance`)

* Launched an EC2 instance named **`devops-nat-instance`** using an **Amazon Linux 2** AMI in `devops-priv-vpc` within `devops-pub-subnet` with **Auto-assign public IP** enabled.
* Configured a custom security group with the following rules:
* **Inbound Rules:** HTTP and HTTPS from the private subnet CIDR range, and SSH from your network's IP range.
* **Outbound Rules:** HTTP and HTTPS destined for `0.0.0.0/0`.


* **Disabled Source/Dest Check:** Navigated to `Instance > Actions > Networking > Change source/destination check` and set it to **Stop/Disable** (critical for NAT instances to forward packets not addressed to them).

### Step 3: Configure OS-Level NAT and IP Tables

* Connected to the NAT instance via SSH and configured IP routing:
```bash
sudo yum install iptables-services -y
sudo systemctl enable iptables
sudo systemctl start iptables

```


* Enabled IP forwarding in the kernel configuration:
```bash
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

```


* Configured `iptables` to masquerade traffic originating from the private subnet (`10.1.1.0/24`):
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.0/24 -j MASQUERADE
sudo service iptables save

```



### Step 4: Update Private Subnet Route Table

* Updated the route table associated with the private subnet (`devops-priv-subnet`) by adding a default route:
* **Destination:** `0.0.0.0/0`
* **Target:** Instance $\rightarrow$ `devops-nat-instance`



---

## Verification & Results

* Verified that internet access was successfully routed from the private EC2 instance through the NAT instance.
* Confirmed successful operation by checking the target S3 bucket (`devops-nat-18335`) to ensure the test file `devops-test.txt` was uploaded successfully.

---

## Key Takeaways

* **Cost-Effective NAT Alternative:** NAT instances provide a budget-friendly alternative to managed NAT Gateways by leveraging standard EC2 instances configured with `iptables` and kernel IP forwarding.
* **Critical NAT Settings:** Disabling source/destination checks on the NAT instance network interface is mandatory; otherwise, AWS packet-filtering will drop all forwarded traffic originating from private instances.
