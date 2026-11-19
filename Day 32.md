# Day 32: Snapshot and Restoration of an RDS Instance

## Overview

In this lab, I created a manual backup snapshot (`devops-snapshot`) of an existing Amazon RDS instance (`devops-rds`) and restored it into a new isolated database instance (`devops-snapshot-restore`) using a `db.t3.micro` instance class. This workflow validates backup integrity and ensures a safe testing environment prior to rolling out database infrastructure updates to production.

---

## Lab Requirements

* **Source RDS Instance:** `devops-rds` (must be in the `Available` state)
* **Snapshot Name:** `devops-snapshot`
* **Restored RDS Instance Name:** `devops-snapshot-restore`
* **Restored Instance Class:** `db.t3.micro`
* **State Requirement:** Restored instance must reach the `Available` state

---

## Step-by-Step Implementation

### Step 1: Create a Manual DB Snapshot (`devops-snapshot`)

* Signed in to the **AWS Management Console** and opened the **Amazon RDS console**.
* In the left navigation pane, chose **Snapshots**.
* Clicked **Take snapshot**.
* Selected **`devops-rds`** from the **DB instance** dropdown, entered **`devops-snapshot`** as the **Snapshot name**, and clicked **Take snapshot**.
* Waited for the manual snapshot to complete creation and transition to the **`Available`** status.

### Step 2: Restore the Snapshot to a New RDS Instance (`devops-snapshot-restore`)

* From the **Snapshots** section in the Amazon RDS console, selected **`devops-snapshot`** and clicked **Actions > Restore snapshot**.
* Under **Settings**, entered **`devops-snapshot-restore`** as the **DB instance identifier**.
* Under **Instance configuration**, selected **Burstable classes (includes t classes)** and chose **`db.t3.micro`**.
* Kept all remaining configurations as default and clicked **Restore DB instance**.
* Monitored the **Databases** dashboard until **`devops-snapshot-restore`** completed provisioning and reached the **`Available`** state.

---

## Verification & Results

* Verified that the manual snapshot **`devops-snapshot`** was created cleanly from `devops-rds` without interrupting active database availability.
* Confirmed that the restored instance **`devops-snapshot-restore`** successfully provisioned on a `db.t3.micro` instance class and reached the **`Available`** state with all source data intact.

---

## Key Takeaways

* **Non-Destructive Testing:** Restoring an RDS snapshot provisions a completely separate database instance with its own endpoint, allowing teams to safely test schema migrations and upgrades without risking production data.
* **Instance Right-Sizing on Restore:** AWS allows changing the DB instance class (e.g., selecting `db.t3.micro`) during snapshot restoration, making it easy to spin up cost-effective staging or validation environments from larger production snapshots.
