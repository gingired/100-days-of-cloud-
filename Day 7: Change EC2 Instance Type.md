Day 7: Changing an EC2 Instance Type

Overview: In this lab, I optimized resource utilization for an underutilized EC2 instance named `devops-ec2`. Because instance types cannot be modified while an instance is running, the workflow required stopping the instance, updating its sizing from `t2.micro` to `t2.nano`, and restarting it to resume operations.


Lab Requirements
- Target Instance: `devops-ec2`
- Initial Instance Type: `t3.micro`
- New Instance Type: `t3.small`
- Final State: Instance must be verified in the **running** state with the updated type.

---

Step-by-Step Implementation

Step 1: Access EC2 Instances
- Open the Amazon EC2 Dashboard and navigate to the **Instances** page to locate the running server.

Step 2: Stop the Instance
- Selected the `devops-ec2` instance.
- Clicked on the Instance state dropdown menu and selected Stop instance.
- Note: Ensured status checks were fully completed before initiating changes.

Step 3: Verify Stopped State
- Confirmed that the instance status column updated to **stopped** before proceeding to configuration changes.

Step 4: Access Change Instance Type Settings
- Clicked on Actions in the top menu bar --> Instance settings --> Change instance type.

Step 5: Update Instance Type
- Selected the new instance type from the dropdown menu (`t3.micro` --> `t3.small`).
- Clicked Apply / Change instance type to save the modification.

Step 6: Restart and Verify
- Returned to the Instance state dropdown and clicked Start instance.
- Verified that the instance successfully transitioned back to the running state and that the instance type displayed as `t3.small`.

---

Verification & Results
- Successfully resized `devops-ec2` to a lighter compute footprint (`t3.small`) while keeping it fully operational.

<img width="1220" height="699" alt="image" src="https://github.com/user-attachments/assets/9402a9cd-f947-4aa0-8920-237af844d78b" />

---

Key Takeaways
- Prerequisite State: An EC2 instance must be in a `stopped` state to modify critical hardware parameters like instance type or root storage attachments.
- Cost & Resource Optimization: Right-sizing instances (such as stepping down from `t3.micro` to `t3.small`) is a fundamental DevOps practice for eliminating cloud waste and optimizing infrastructure budgets.
