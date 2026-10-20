Day 3: Creating an AWS Subnet

Overview: In this lab, I created a custom subnet named `datacenter-subnet` under the default Virtual Private Cloud (VPC) on Amazon Web Services (AWS) using the VPC Dashboard. Subnets partition the VPC's IP address range, allowing resources like EC2 instances to be launched into specific Availability Zones with designated network boundaries.

---

Lab Requirements

- VPC: Default VPC
- Subnet Name: `datacenter-subnet`
- Availability Zone: Selected a preferred Availability Zone (e.g., `us-east-1a`)
- IPv4 CIDR Block: Chosen carefully to avoid overlapping with any existing subnets in the default VPC.

---

Step-by-Step Implementation

Step 1: Navigate to the VPC Dashboard

- Logged in to the AWS Management Console, searched for VPC, and opened the VPC Dashboard.

Step 2: Access Subnets and Select the Default VPC

- In the left sidebar navigation menu, clicked on Subnets.
- Ensured the correct Default VPC was selected for association.

Step 3: Configure Subnet Settings

- Clicked Create subnet.
- Config

- <img width="1211" height="460" alt="image" src="https://github.com/user-attachments/assets/4b1f8f60-0a6d-42fb-a4fe-41b0dbfe13da" />
