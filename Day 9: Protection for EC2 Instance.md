Day 9: Enable Termination Protection for EC2 Instance

Overview: In this lab, I enabled Termination Protection on an Amazon EC2 instance named nautilus-ec2 under the us-east-1 region. Enabling termination protection prevents accidental or unauthorized deletion of vital cloud resources, ensuring that critical instances cannot be accidentally terminated.


Lab Requirements

Target Instance: nautilus-ec2
Region: us-east-1 (N. Virginia)
Action: Enable instance termination protection


Step-by-Step Implementation

Step 1: Navigate to EC2 Instances
Open the Amazon EC2 Dashboard and click on Instances running under the resource section to view the server inventory.

Step 2: Access Termination Protection Settings
Selected the target instance (nautilus-ec2).
Clicked on Actions in the top menu bar $\rightarrow$ Instance settings $\rightarrow$ Change termination protection.

Step 3: Enable and Save

Checked the box to Enable termination protection.
Clicked Save to apply the configuration.

Verification & Results

Confirmed that termination protection was successfully enabled, securing the instance from accidental deletion.

<img width="1222" height="424" alt="image" src="https://github.com/user-attachments/assets/d23d6449-fd85-4179-81b8-a0335f3070bf" />


Key Takeaways

Protection Against Data Loss: Termination protection is an essential safety feature in cloud architectures that safeguards production instances from being permanently deleted by user error.
Modification Requirement: If an instance with termination protection enabled needs to be decommissioned, the protection setting must first be disabled via the console or CLI before the termination action can succeed.
