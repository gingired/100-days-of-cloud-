Day 13: Create AMI from EC2 Instance

Overview: In this lab, I created a custom Amazon Machine Image (AMI) named `devops-ec2-ami` from an active EC2 instance named `devops-ec2` in the `us-east-1` region. AMIs serve as standardized golden images or master templates, capturing the instance's underlying EBS root volume state, installed software, and configurations to enable rapid, repeatable provisioning across infrastructure.

Lab Requirements

Source Instance: `devops-ec2` (`i-08f385e2ac174c3d3`)
Target AMI Name: `devops-ec2-ami`
Region: `us-east-1` (N. Virginia)
Target Status: `Available`
Action: Create a custom AMI from the running instance and verify image availability

Step-by-Step Implementation

Step 1: Navigate to EC2 Instances
Opened the Amazon EC2 Console and navigated to Instances --> Instances.
Located and selected the target instance `devops-ec2` (`i-08f385e2ac174c***`).

Step 2: Initiate Image Creation
With `devops-ec2` selected, clicked **Actions $\rightarrow$ Image and templates --> Create image**.

Step 3: Configure AMI Settings
Entered `devops-ec2-ami` in the Image name field.
(Optional) Added a descriptive string in the **Image description** field (e.g., Golden AMI created from devops-ec2 for Day 13 lab).
Left No reboot unchecked (default behavior) to ensure file system consistency while the underlying EBS snapshots were taken.
Verified that all associated block device storage mappings (root volume and secondary attached EBS data volumes like `/dev/sdb`) were included.

Step 4: Finalize & Create Image

Clicked Create image to launch the AMI creation process and initiate background snapshot generation.

Step 5: Verification & Results
Navigated to Images --> AMIs in the left navigation pane.
Located `devops-ec2-ami` and monitored its status until it transitioned from `pending` to `available`.

Key Takeaways

Golden Images for Automation: AMIs capture complete system states (OS, application code, data volumes), allowing rapid horizontal scaling or auto-scaling launch templates without manual post-provisioning setup.
Snapshot Underpinnings: When an AMI is created, AWS automatically creates backing EBS snapshots for each attached block storage drive.
Reboot Behavior:** By default, AWS reboots the target EC2 instance during AMI creation to flush the file system buffer and ensure data integrity across attached EBS volumes.

<img width="1236" height="403" alt="image" src="https://github.com/user-attachments/assets/6ae3526b-a76e-4d15-95ec-7f80ad1dc603" />
