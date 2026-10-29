Day 12: Attach EBS Volume to EC2 Instance

Overview: In this lab, I attached a secondary Amazon EBS data volume to an Amazon EC2 instance (`devops-ec2` / `xfusion-ec2`) in the `us-east-1` region. Attaching secondary volumes provides scalable storage capacity for workloads beyond the root OS filesystem.

Lab Requirements
Target Instance: `devops-ec2` (`i-08f385e2ac174c3d3`)
Target Volume: `vol-02f330379f7aa7d04` (100 GiB gp3)
Region: `us-east-1` (N. Virginia)
Device Name: `/dev/sdb`
Action: Attach the EBS volume to the EC2 instance and verify state

Step-by-Step Implementation

Step 1: Navigate to EBS Volumes
Opened the Amazon EC2 Console and navigated to **Elastic Block Store $\rightarrow$ Volumes**.

Step 2: Identify and Match Availability Zones
Verified the deployment Availability Zone of the target EC2 instance (`devops-ec2` in `us-east-1c`).
Created a new 100 GiB `gp3` storage volume (`vol-02f330379f7******`), explicitly selecting `use1-az4 (us-east-1c)` as the Availability Zone to ensure strict physical alignment with the target server.

Step 3: Attach Volume to Instance
Selected `vol-02f330379f7******` from the volumes list (ensuring its status was `Available`).
Clicked Actions --> Attach volume.
Selected `i-08f385e2ac174****` (`devops-ec2`) from the instance list.
Set the custom device mapping path to `/dev/sdb`.
Clicked Attach volume.

Challenges & Troubleshooting

1. Availability Zone Mismatch (`us-east-1a` vs `us-east-1c`)
Struggle: When attempting to attach an existing volume (`datacenter-volume`), the target instance was missing from the selection dropdown, showing the warning: "No matching running or stopped instances in use1-az1 (us-east-1a)".
Root Cause: EBS volumes are strictly isolated to the specific Availability Zone in which they are created. The existing volume resided in `us-east-1a`, whereas `devops-ec2` was deployed in `us-east-1c`.
Resolution: Understood the physical constraint, created a new volume specifically in `use1-az4 (us-east-1c)` to match the instance's zone, allowing AWS to locate and display the instance.

2. Attempting to Attach an Already Attached (`In-use`) Volume

Struggle: The "Attach volume" option was greyed out / unclickable despite selecting a volume in the console.
Root Cause: Selected `vol-0bc4b85a816******` which was already in an `In-use` state attached to another instance. Standard EBS volumes can only be attached to a single instance at a time (unless Multi-Attach is enabled on specific volume types).
Resolution: Unchecked the `In-use` volume and selected the newly created, `Available` 100 GiB volume (`vol-02f330379f7******`).

Verification & Results
Received confirmation message: *"Successfully attached volume vol-02f330379f7aa7d04 to instance i-08f385e2ac1******"*.
Confirmed the volume state transitioned from `Available` to `In-use`.

Key Takeaways
AZ Bound: EBS volumes cannot cross Availability Zone boundaries within a region; volumes and EC2 instances must share the exact same AZ.
Volume States: Always verify that an EBS volume state is `Available` before attempting attachment.
AZ Names vs AZ IDs: `us-east-1c` corresponds to AZ ID `use1-az4` in standard AWS account mappings.

<img width="1241" height="531" alt="image" src="https://github.com/user-attachments/assets/146e0985-19bc-44b9-bfdf-9ea544852bb3" />
