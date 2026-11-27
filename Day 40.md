# Day 40: Troubleshooting Internet Accessibility for an EC2-Hosted Application

## Overview

In this lab, I diagnosed and resolved a connectivity issue preventing external internet access on port `80` to an Nginx web application running on `nautilus-ec2` inside `nautilus-vpc`. Although security group `nautilus-sg` permitted port `80` traffic, the subnet's route table (`nautilus-rtb`) had a `Blackhole` internet gateway route because `nautilus-ig` was in a `Detached` state. Attaching `nautilus-ig` to `nautilus-vpc` restored the route to `Active` and made the Nginx application reachable over the internet.

---

## Lab Requirements

* **VPC Name:** `nautilus-vpc`
* **Subnet & Route Table:** `nautilus-subnet` associated with route table `nautilus-rtb`
* **Internet Gateway:** `nautilus-ig` (attached to `nautilus-vpc` so routes show `Active` instead of `Blackhole`)
* **EC2 Instance & Security Group:** `nautilus-ec2` running Nginx with `nautilus-sg` allowing port `80`
* **Verification:** Nginx welcome page accessible via `nautilus-ec2`'s public IP address in a browser

---

## Step-by-Step Implementation

### Step 1: Inspect VPC Topology, Route Table, and Internet Gateway

* Inspected the `nautilus-vpc` resource map and identified that internet gateway routing to `nautilus-subnet` via `nautilus-rtb` was not functioning properly, causing the subnet to behave like a private subnet.
* Checked the routes in `nautilus-rtb` and saw the internet gateway target in a **`Blackhole`** state rather than `Active`.
* Navigated to **Internet Gateways** in the VPC navigation pane and confirmed that the internet gateway was **`Detached`** and not associated with `nautilus-vpc`.

### Step 2: Attach the Internet Gateway (`nautilus-ig`) to `nautilus-vpc`

* Selected **`nautilus-ig`** under **Internet Gateways**, clicked **Actions > Attach to VPC**, chose **`nautilus-vpc`**, and attached it.
* Verified in `nautilus-rtb` that the internet gateway route status updated to **`Active`**.
* Reviewed the **`nautilus-vpc`** resource map to confirm the Internet Gateway connection was in place.

---

## Verification & Results

* Opened the **EC2 Dashboard**, selected **`nautilus-ec2`**, and navigated to its public IP address in a web browser.
* Confirmed that the default **Nginx** welcome page loaded over port `80`.

---

## Key Takeaways

* **Diagnosing `Blackhole` Routes:** A `Blackhole` target status in a route table indicates that the referenced gateway or peering target is missing, invalid, or detached—meaning traffic cannot traverse that path even when security groups allow the port.
* **Complete Public Subnet Path:** A subnet requires both a `0.0.0.0/0` route pointing to an **attached** Internet Gateway in its associated route table and appropriate security group rules before public IP traffic can reach an EC2 instance.
