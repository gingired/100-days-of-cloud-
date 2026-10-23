Day 6: Launching an Amazon EC2 Instance

Overview: In this lab, I launched a virtual server—an Amazon Elastic Compute Cloud (EC2) instance—on Amazon Web Services (AWS) using the EC2 console. This brings together the compute power, AMIs, key pairs, and security configurations needed to run cloud-based workloads.

Lab Requirements
- Instance Name: `devops-ec2`
- AMI (Operating System): Amazon Linux AMI
- Instance Type: `t2.micro`
- Key Pair: Created a new RSA key pair named `devops-kp`
- Security Group: Attached the default security group

---

Step-by-Step Implementation

Step 1: Navigate to EC2 Console
- Log in to the AWS Management Console and open the EC2 Dashboard.

Step 2: Initiate Instance Launch
- From the EC2 dashboard, clicked on Launch instance.

Step 3: Configure Instance Name
- Under Name and tags, set the instance name to `devops-ec2`.

Step 4: Choose Application and OS Image (AMI)
- Selected the Amazon Linux AMI as the base operating system image for the instance.

Step 5: Select Instance Type
- Chose `t2.micro` as the instance type (eligible for the AWS Free Tier).

Step 6: Configure Key Pair
- Under Key pair, created a new RSA key pair named `devops-kp` and securely downloaded the private key file.

Step 7: Network Settings
- Kept the default VPC and automatic subnet selection.
- Attached the **default security group** as specified by the lab requirements.

Step 8: Review and Launch
- Reviewed the summary configuration panel and clicked Launch instance to provision the server.

Verification & Results
- Clicked on the instance ID from the green success notification to monitor the launch status and ensure the instance reached the **running** state successfully.

![EC2 Instance Launch Confirmation](https://github.com/user-attachments/assets/your-uploaded-image-id-here)

Key Takeaways
- EC2 Lifecycle: Once an instance is launched, it cycles through initialization states (pending --> running) and can be stopped, started, or terminated as needed.
- Prerequisites Connection: Combining a proper AMI, a `t2.micro` compute footprint, an assigned key pair (`devops-kp`), and a security group establishes the exact foundation needed to securely connect via SSH.

<img width="1223" height="260" alt="image" src="https://github.com/user-attachments/assets/761595c0-f952-48dc-9ef0-3c1919ebb23a" />
