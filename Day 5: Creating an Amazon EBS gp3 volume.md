Day 5: Creating an Amazon EBS gp3 Volume

Overview: In this lab, I created a General Purpose SSD (`gp3`) Amazon Elastic Block Store (EBS) volume on Amazon Web Services (AWS) using the EC2 console. EBS volumes provide persistent block storage blocks that can be attached to EC2 instances as virtual hard drives.

---

Lab Requirements
- Volume Name: `datacenter-volume`
- Volume Type: `gp3` (General Purpose SSD)
- Volume Size: `2 GiB`

---

Step-by-Step Implementation

Step 1: Navigate to the EC2 Console
- Log in to the AWS Management Console and open the EC2 Dashboard.

Step 2: Access EBS Volumes
- In the left-hand navigation pane, scrolled down to Elastic Block Store and clicked on Volumes.
- Clicked on Create volume.

Step 3: Configure Volume Settings
- Configured the storage parameters to meet the lab requirements:
  - Volume type: Selected **General Purpose SSD (gp3).
  - Size: Set to `2` GiB.
  - IOPS / Throughput:** Kept the default baselines for gp3 (3000 IOPS and 125 MiB/s).
  - **Availability Zone:** Chose the target Availability Zone (ensuring it matches the intended EC2 instance).

Step 4: Create and Tag the Volume
- Clicked create volume to provision the storage.
- Once created, clicked the pencil (edit) icon next to the volume to add a Name tag with the value `datacenter-volume`.

---

Verification & Results
- Verified that the volume state changed to **available** and that it was correctly tagged as `datacenter-volume` with a size of 2 GiB and gp3 volume type.

  ![EBS Volume Creation Confirmation](https://github.com/user-attachments/assets/your-uploaded-image-id-here)

---

Key Takeaways
- gp3 vs gp2: `gp3` allows users to independently scale IOPS and throughput without needing to provision extra storage capacity, making it more cost-effective than older `gp2` volumes.
- Availability Zone Rule: An EBS volume can only be attached to an EC2 instance that resides within the exact same Availability Zone.

<img width="1223" height="610" alt="image" src="https://github.com/user-attachments/assets/511ea100-e953-4ee4-ae82-5142ec7cd534" />

