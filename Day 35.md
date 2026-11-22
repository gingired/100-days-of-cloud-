# Day 35: Deploying and Managing Applications on AWS

## Overview

In this lab, I provisioned a private Amazon RDS MySQL instance (`nautilus-rds`) running MySQL `8.4.5` on a `db.t3.micro` instance with `5GiB` of `gp2` storage and an initial database named `nautilus_db`. I also configured security groups, established password-less SSH access from `aws-client` to `nautilus-ec2`, and deployed a PHP database connection script (`/var/www/html/index.php`) to verify application-to-database connectivity.

---

## Lab Requirements

* **RDS Instance Name:** `nautilus-rds` (private, `Available` state)
* **Template / Instance Class:** Sandbox template, `db.t3.micro`
* **Engine & Version:** MySQL `v8.4.5`
* **Credentials & Database:** Master username `nautilus_admin`, initial database `nautilus_db`
* **Storage:** `gp2`, `5GiB`
* **EC2 Instance:** `nautilus-ec2` with HTTP (port `80`) accessible and MySQL (port `3306`) allowed to RDS
* **Application Verification:** `/var/www/html/index.php` deployed on `nautilus-ec2` displaying `Connected successfully` when accessed via public IP

---

## Step-by-Step Implementation

### Step 1: Adjust Security Group Rules for `nautilus-ec2` and RDS

* Opened the **EC2 Console** and selected **Security groups** under **Network & Security**.
* Selected the security group associated with `nautilus-ec2`.
* Edited **Inbound rules** to open **HTTP (Port 80)** and ensure required access is permitted, and configured RDS inbound rules so `nautilus-ec2` can connect on **MySQL/Aurora (Port 3306)**.

### Step 2: Enable Password-Less SSH Access from `aws-client` to `nautilus-ec2`

* Generated an SSH key pair on `aws-client` if one did not already exist:
```bash
ssh-keygen -t rsa

```


* Connected to `nautilus-ec2` from the AWS Management Console using **EC2 Instance Connect**.
* Switched to the `root` user, navigated to `/root`, and added the contents of `/root/.ssh/id_rsa.pub` from `aws-client` into `/root/.ssh/authorized_keys`:
```bash
sudo su -
cd ~
nano .ssh/authorized_keys

```


* Verified password-less SSH access from `aws-client` as `root`:
```bash
ssh root@<nautilus-ec2-pub-ip>

```



### Step 3: Create the Private RDS Instance (`nautilus-rds`)

* Opened the **Amazon RDS Console** and selected **Create database** using **Full configuration**.
* Under **Engine options**, selected **MySQL** and engine version **`8.4.5`**.
* Selected the appropriate template/tier (sandbox template) and **Single-AZ DB instance deployment (1 instance)**.
* Under **Settings**, entered **`nautilus-rds`** as the DB instance identifier, **`nautilus_admin`** as the master username, and configured the master password.
* Under **Instance configuration**, selected **Burstable classes (includes t classes)** and **`db.t3.micro`**.
* Under **Storage**, selected **`gp2`** and allocated **`5`** GiB.
* Under **Connectivity**, selected **Connect to an EC2 compute resource**, chose **`nautilus-ec2`**, and kept remaining options as default.
* Under **Additional configuration**, entered **`nautilus_db`** for the **Initial database name** and clicked **Create database**.

### Step 4: Configure and Deploy `index.php` to `nautilus-ec2`

* Updated `/root/index.php` on `aws-client` with the RDS endpoint (`$dbhost`), database name (`nautilus_db`), username (`nautilus_admin`), and password:
```php
<?php
 $dbname = 'nautilus_db';
 $dbuser = 'nautilus_admin';
 $dbpass = '<your-rds-password>';
 $dbhost = '<your-nautilus-rds-endpoint>';

 $link = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");
 mysqli_select_db($link, $dbname) or die("Could not open the db '$dbname'");

 $test_query = "SHOW TABLES FROM $dbname";
 $result = mysqli_query($link, $test_query);

 $tblCnt = 0;
 while($tbl = mysqli_fetch_array($result)) {
   $tblCnt++;
 }

 if (!$tblCnt) {
   echo "Connected successfully<br />\n";
 } else {
   echo "Connected successfully<br />\n";
 }
?>

```


* Copied `index.php` from `aws-client` to `nautilus-ec2` under `/var/www/html/`:
```bash
scp /root/index.php root@<nautilus-ec2-pub-ip>:/var/www/html/index.php

```


* On `nautilus-ec2`, set proper file ownership and permissions, removed the default `index.html`, and restarted Apache:
```bash
sudo chown www-data:www-data /var/www/html/index.php
sudo chmod 644 /var/www/html/index.php
sudo rm /var/www/html/index.html
sudo systemctl restart apache2

```



---

## Verification & Results

* Confirmed `nautilus-rds` reached the **`Available`** state in the Amazon RDS console.
* Accessed `http://<nautilus-ec2-pub-ip>` in a web browser and verified that `Connected successfully` was displayed.

---

## Key Takeaways

* **End-to-End Tier Connectivity:** Connecting a web application server (`nautilus-ec2`) to a private RDS instance requires aligned security group rules (port `80` for HTTP clients and port `3306` for MySQL between the app tier and database tier) plus accurate endpoint and credential configuration.
* **Web Server File & Service Hygiene:** Setting proper `www-data` ownership and removing the default `index.html` ensures Apache serves the PHP application cleanly without fallback conflicts.
