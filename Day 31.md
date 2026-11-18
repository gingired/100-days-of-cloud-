# Day 31: Configuring a Private RDS Instance for Application Development

## Overview

In this lab, I provisioned a private Amazon RDS MySQL database instance named **`datacenter-rds`** configured with a sandbox template, a `db.t3.micro` instance class, and MySQL engine version `8.4.x`. Additionally, I enabled storage autoscaling with a maximum threshold of `50GB` to ensure optimal resource scalability and cost management for application development.

---

## Lab Requirements

* **DB Instance Name:** `datacenter-rds`
* **Template:** Sandbox template
* **Instance Class:** `db.t3.micro`
* **Engine Configuration:** MySQL (Version `8.4.x`)
* **Storage Autoscaling:** Enabled with a threshold limit of `50GB`
* **State Requirement:** Instance must reach the `available` state

---

## Step-by-Step Implementation

### Step 1: Create the MySQL Database Instance

* Signed in to the **AWS Management Console** and opened the **Amazon RDS console**.
* In the navigation pane, chose **Databases**, then clicked **Create database** using the **Easy create** method.
* Selected **MySQL** under the configuration options.
* Selected **Sandbox** for the template/instance size configuration.
* Entered **`datacenter-rds`** into the **DB instance identifier** field.
* Clicked **Create database** to initiate provisioning.

### Step 2: Modify Instance Class, Version, and Enable Storage Autoscaling

* Once the database was created, selected the instance and clicked **Modify**.
* Updated the **DB engine version** to `8.4.+` (MySQL 8.4.x).
* Under the **Instance configuration** section, selected **`db.t3.micro`** as the instance class.
* Navigated to **Storage > Additional storage configuration**, checked **Storage autoscaling**, and set the maximum storage threshold to **`50GB`**.
* Reviewed the modification summary at the bottom of the page and clicked **Continue**.
* Under **Scheduling modifications**, selected **Apply immediately** and clicked **Modify DB instance**.

---

## Verification & Results

* Monitored the RDS console until the database status transitioned from *Modifying* to **`Available`**.
* Confirmed that `datacenter-rds` is running MySQL 8.4.x on a `db.t3.micro` instance with storage autoscaling capped at 50GB.

---

## Key Takeaways

* **Dynamic Resource Scaling:** Enabling storage autoscaling prevents application downtime caused by unexpected database storage exhaustion.
* **Sandbox Environment Best Practices:** Utilizing lightweight instance classes like `db.t3.micro` with proper engine version targeting ensures cost efficiency during early-stage application development.
