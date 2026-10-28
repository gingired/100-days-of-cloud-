Day 11: Attach Elastic Network Interface to EC2 Instance

Overview

In this lab, I attached an Elastic Network Interface named `devops-eni` to an Amazon EC2 instance named `devops-ec2` in the us-east-1 region. Attaching a secondary network interface allows an instance to handle multi-homed networking, distinct subnet traffic routing, or dedicated security boundaries.

Lab Requirements

* Target Instance: `devops-ec2`
* Target Network Interface: `devops-eni`
* Region: us-east-1 (N. Virginia)
* Action: Attach the network interface and verify the status is attached

Step-by-Step Implementation

Step 1: Navigate to Network Interfaces
Open the Amazon EC2 console and select Network Interfaces in the navigation pane.

Step 2: Troubleshoot Availability Zone and Create a Compatible ENI
Initially encountered attachment constraints and instance visibility limitations due to availability zone alignment or pre-existing state constraints.
Created a brand-new custom Elastic Network Interface, ensuring its Subnet and Availability Zone matched the exact deployment zone of `devops-ec2` to guarantee compatibility.
Set the description to `devops-eni`, left the interface type as ENA, and kept private IPv4 address allocation set to auto-assign.

Step 3: Select Network Interface
Selected the checkbox for the newly created compatible `devops-eni` network interface from the console.

Step 4: Configure Attachment
Clicked on Actions --> Attach.
Entered or selected the target instance (`devops-ec2`) from the instance selection list.

Step 5: Finalize Attachment
Clicked Attach to bind the network interface to the EC2 instance.

Verification & Results
Confirmed that the network interface status successfully changed to attached before completing the task.

Key Takeaways:
Flexible Networking: Elastic Network Interfaces (ENIs) enable complex network configurations, such as separating management traffic from public application traffic on a single virtual server.
Availability Zone Constraints: Just like EBS volumes, an ENI must reside in the exact same Availability Zone as the EC2 instance it is being attached to; creating a replacement ENI in a matching subnet resolves visibility or zone mismatch hurdles during attachment.

<img width="1234" height="482" alt="image" src="https://github.com/user-attachments/assets/4280c6b3-efcf-4ca9-b592-c3d66c4787c6" />
