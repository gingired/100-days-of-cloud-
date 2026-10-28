Day 10: Attach Elastic IP to EC2 Instance

Overview: In this lab, I associated an Elastic IP address named `devops-ec2-eip` with an Amazon EC2 instance named `devops-ec2` in the us-east-1 region. Attaching an Elastic IP ensures that the instance retains a permanent, fixed public IPv4 address even if the instance is stopped and restarted.

Lab Requirements
Target Instance: `devops-ec2`
Target Elastic IP: `devops-ec2-eip`
Region: us-east-1 (N. Virginia)
Action: Associate the Elastic IP address with the EC2 instance

Step-by-Step Implementation

Step 1: Navigate to Elastic IPs
Open the Amazon EC2 console and select Elastic IPs in the navigation pane.

Step 2: Select Elastic IP and Choose Associate Action
Selected the `devops-ec2-eip` Elastic IP address.
Clicked on Actions in the top menu bar $\rightarrow$ Associate Elastic IP address.

Step 3: Configure Resource Association
For Resource type, chose Instance.
For Instance, searched for and selected the `devops-ec2` instance.
Left Private IP address settings at default.

Step 4: Finalize Association
Clicked Associate to bind the Elastic IP to the running EC2 instance.


Verification & Results
Confirmed that the Elastic IP address was successfully linked to the target instance, granting it a permanent public IP for inbound and outbound traffic connectivity.

Key Takeaways
Permanent Public Addressing: Associating an EIP overrides dynamic public IP assignments, ensuring consistent endpoint resolution for external clients or management scripts.
Association Prerequisite: An Elastic IP must first be allocated (as performed in Day 4) before it can be associated with an active compute resource or network interface.

<img width="1224" height="333" alt="image" src="https://github.com/user-attachments/assets/38348903-5065-464e-b335-7f6e0bfc3bfa" />
