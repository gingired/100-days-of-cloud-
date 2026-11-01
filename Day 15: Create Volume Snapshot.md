# Day 15: Create Volume Snapshot

## Overview

In this lab, I created a point-in-time backup snapshot of an existing Amazon EBS storage volume named `datacenter-vol` in the `us-east-1` region. EBS snapshots provide incremental backup protection, capturing the exact state of block storage devices to support data recovery and volume cloning.

---

## Lab Requirements

* **Target Volume:** `datacenter-vol` (`vol-0c314913cfa925fe4`)
* **Snapshot Name:** `datacenter-vol-ss`
* **Snapshot Description:** `datacenter Snapshot`
* **Region:** `us-east-1` (N. Virginia)
* **Target State:** `completed`
* **Action:** Generate an EBS volume snapshot, apply appropriate tags/names, and verify completion status

---

## Step-by-Step Implementation

### Step 1: Navigate to EC2 Snapshots

* Opened the Amazon EC2 Console and navigated to **Elastic Block Store $\rightarrow$ Snapshots**.
* Clicked on **Create snapshot**.

### Step 2: Configure Snapshot Resource Settings

* For **Resource type**, selected **Volume**.
* Under **Volume ID**, searched for and selected **`datacenter-vol`** (`vol-0c314913cfa925fe4`).

### Step 3: Add Description and Tags

* In the **Description** field, entered **`datacenter Snapshot`**.
* Added a custom tag with Key `Name` and Value **`datacenter-vol-ss`** (or named the snapshot directly from the console Name column after creation).

### Step 4: Finalize Snapshot Creation

* Clicked **Create snapshot** to trigger the backup process.

---

## Verification & Results

* Navigated back to the **Snapshots** dashboard.
* Located **`datacenter-vol-ss`** and monitored its progress until the status transitioned from `pending` to **`completed`**.

---

## Key Takeaways

* **Incremental Backups:** EBS snapshots are incremental, meaning only the blocks changed since your last snapshot are saved, reducing storage costs while ensuring comprehensive backup points.
* **Point-in-Time Data Safety:** Creating snapshots of critical data volumes protects workloads from accidental data corruption or loss before performing major structural modifications.

<img width="1228" height="323" alt="image" src="https://github.com/user-attachments/assets/b0bc7af3-5297-4dde-bdaf-8f4e1cecbafc" />
