Day 8: Enable Stop Protection for EC2 Instance

Overview: In this lab, I enabled Stop Protection on an Amazon EC2 instance named datacenter-ec2 under the us-east-1 region. Enabling stop protection prevents accidental or unauthorized shutdown of critical cloud infrastructure, adding an extra layer of safeguard for production environments.

Lab Requirements
Target Instance: datacenter-ec2
Region: us-east-1 (N. Virginia)
Action: Enable instance stop protection

Step-by-Step Implementation

Step 1: Navigate to EC2 Instances

Open the Amazon EC2 Dashboard and click on Instances (or the running instances resource count) to view the server inventory.

Step 2: Access Stop Protection Settings

Selected the target instance (datacenter-ec2).
Clicked on Actions in the top menu bar --> Instance settings --> Change stop protection.

Step 3: Enable and Save

Checked the box to Enable stop protection.
Clicked Save to apply the configuration.

Verification & Results

Confirmed that stop protection was successfully enabled, ensuring the instance is protected from accidental stops.


Key Takeaways

Accidental Termination & Stop Prevention: Protection features in AWS act as fail-safes against human error, ensuring vital virtual servers cannot be accidentally halted or deleted.
Prerequisite for Modifications: If an instance has stop protection enabled, you must explicitly disable the protection toggle before you are allowed to stop or resize the instance.

<img width="1205" height="403" alt="image" src="https://github.com/user-attachments/assets/155459cc-0f82-49aa-a94e-88030ebfa83e" />
