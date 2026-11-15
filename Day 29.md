# Day 29: Establishing Secure Communication Between Public and Private VPCs via VPC Peering

## Overview

In this lab, I established secure cross-VPC communication by setting up a **VPC Peering Connection** between the default public VPC (containing `xfusion-public-ec2`) and a private VPC (`xfusion-private-vpc` with CIDR `10.1.0.0/16`, containing `xfusion-private-subnet` and `xfusion-private-ec2`), enabling seamless network traversal and ICMP connectivity between public and private instances.

---

## Lab Requirements

* **Public Instance:** `xfusion-public-ec2` (existing in the default public VPC)
* **Private VPC:** `xfusion-private-vpc` (`10.1.0.0/16`)
* **Private Subnet:** `xfusion-private-subnet` (`10.1.1.0/24`)
* **Private Instance:** `xfusion-private-ec2`
* **VPC Peering Connection:** `xfusion-vpc-peering`
* **Routing & Security:** Cross-VPC route table updates and ICMP/SSH security group configurations.

---

## Step-by-Step Implementation

### Step 1: Create and Accept the VPC Peering Connection

* Navigated to the **VPC Console $\rightarrow$ Peering connections** and clicked **Create peering connection**.
* Named the peering connection **`xfusion-vpc-peering`**.
* Selected the Requester VPC (Default VPC) and the Accepter VPC (`xfusion-private-vpc` in this Region/Account).
* Clicked **Create peering connection** and subsequently **Accept VPC peering connection request**.
* Selected the peering connection, navigated to **Actions > Edit DNS settings**, and enabled DNS resolution for both requester and accepter sides.

### Step 2: Configure Route Tables

* Updated the route tables for both the default public VPC and `xfusion-private-vpc` to point each other's CIDR blocks to the peering connection `xfusion-vpc-peering`:
* **Default VPC Route Table:** Added route for `10.1.0.0/16` targeting `xfusion-vpc-peering`.
* **Private VPC Route Table:** Added route for the default VPC CIDR targeting `xfusion-vpc-peering`.



### Step 3: Configure Security Group Inbound Rules for ICMP

* Updated the security group for `xfusion-public-ec2` to allow **ICMP** traffic with the private VPC's CIDR (`10.1.0.0/16`) as the source.
* Updated the security group for `xfusion-private-ec2` to allow **ICMP** traffic with the public VPC's CIDR as the source.

### Step 4: Enable SSH Access to the Public Instance

* Updated the security group of `xfusion-public-ec2` to permit **SSH (Port 22)** traffic from Anywhere (`0.0.0.0/0`).
* Added the client public key (`/root/.ssh/id_rsa.pub`) to the `ec2-user`'s `authorized_keys` file on `xfusion-public-ec2`.
* Verified SSH connectivity into the public instance:
```bash
ssh ec2-user@<public-ip-of-xfusion-public-ec2>

```



---

## Verification & Results

* Connected to `xfusion-public-ec2` via SSH.
* Executed a ping command from the public instance to the private instance using its private IP address, confirming successful packet delivery and cross-VPC peering functionality:
```bash
ping <private-ip-of-xfusion-private-ec2>

```



---

## Key Takeaways

* **VPC Peering Isolation:** VPC peering creates a direct network bridge between two separate VPCs using private IP addresses without traversing the public internet.
* **Bidirectional Routing & Security:** Establishing peering requires synchronized configuration on three levels: **Peering Acceptance/DNS Settings**, **Route Table Entries** on both sides, and **Security Group Rules** allowing specific traffic (such as ICMP).
