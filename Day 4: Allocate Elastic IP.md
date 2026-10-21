Day 4: Allocate an Elastic IP

Overview: In this lab, I allocated a static public IPv4 address—known as an Elastic IP (EIP)—in Amazon Web Services (AWS) using the VPC console. Unlike standard public IP addresses that change when an instance is stopped and started, an Elastic IP remains persistent, making it ideal for managing external access to cloud servers.

---

Lab Requirements
- Resource Type: Elastic IP Address (EIP)
- Pool: Amazon's pool of IPv4 addresses
- Name Tag: `nautilus-eip`

---

Step-by-Step Implementation

Step 1: Navigate to the VPC Console
- Log in to the AWS Management Console, search for VPC, and open the **VPC Console**.

Step 2: Access Elastic IPs
- In the left-hand navigation pane, clicked on Elastic IPs.

Step 3: Initiate Allocation
- Clicked on Allocate Elastic IP address.

Step 4: Configure Public IPv4 Address Settings
- Left the network border group at its default setting.
- Under Public IPv4 address pool**, selected Amazon's pool of IP addresses.

Step 5: Add Tags (Optional for Lab)
- Added a tag to identify the resource:
  - Key: `Name`
  - Value:*`nautilus-eip`

Step 6: Allocate and Rename
- Clicked Allocate to provision the Elastic IP.
- Clicked the pencil (edit) icon next to the allocated EIP to explicitly name/tag it as `nautilus-eip`.

---

Verification & Results
- Confirmed that the Elastic IP was successfully allocated and displayed in the Elastic IPs dashboard with the proper name tag and static public IP assigned.

  ![Elastic IP Allocation Confirmation](https://github.com/user-attachments/assets/your-uploaded-image-id-here)

---

Key Takeaways
- Static vs. Dynamic Public IPs: Standard public IPs assigned to an EC2 instance change upon a stop/start cycle. Elastic IPs provide a fixed public address that can be remapped to another instance in case of failure.
- Cost Note: AWS allocates Elastic IPs free of charge as long as they are actively associated with a running EC2 instance. Unattached EIPs incur a small hourly fee to prevent IP hoarding.

<img width="1212" height="389" alt="image" src="https://github.com/user-attachments/assets/9dde4457-000c-41bc-a898-17dc1aa1fd30" />
