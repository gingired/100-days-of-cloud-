# Day 22: Configuring Secure SSH Access to an EC2 Instance

## Overview

In this lab, I provisioned a new Amazon EC2 instance named **`nautilus-ec2`** of type `t3.micro` and configured secure, passwordless remote SSH access from the client host using a custom-generated RSA key pair named **`keypair`**.

---

## Lab Requirements

* **Instance Name:** `nautilus-ec2`
* **Instance Type:** `t3.micro`
* **Key Pair Name:** `keypair`
* **SSH Key Path:** `~/keypair.pub` (Home directory)
* **Access Goal:** Enable secure, passwordless root and user-level SSH connectivity from the client host to the EC2 instance

---

## Step-by-Step Implementation

### Step 1: Generate RSA Key Pair on Client Host

* Opened the terminal on the client host.
* Generated a new RSA key pair and saved it as `keypair` using the command:
```bash
ssh-keygen -t rsa

```


* Displayed and copied the public key content from the home directory using:
```bash
cat ~/keypair.pub

```



### Step 2: Import Public Key into AWS EC2

* Navigated to the **EC2 Console $\rightarrow$ Key Pairs**.
* Clicked **Actions $\rightarrow$ Import key pair**.
* Named the key pair **`keypair`** and pasted the public key contents (`~/keypair.pub`).

### Step 3: Launch EC2 Instance with Imported Key

* Launched a new `t3.micro` instance named **`nautilus-ec2`** (IP: `3.89.180.178`) from the EC2 dashboard.
* Selected the imported **`keypair`** during the instance launch wizard configuration.

### Step 4: Establish Initial SSH Connection

* Restricted private key permissions on the local host to satisfy SSH security requirements:
```bash
chmod 400 keypair

```


* Initiated an initial connection from the client terminal using the private key:
```bash
ssh -i keypair ec2-user@3.89.180.178

```



### Step 5: Configure Root Authorized Keys

* Switched to root mode on the EC2 instance:
```bash
sudo -i

```


* Created and opened the `/root/.ssh/authorized_keys` file and appended the client's public key content to permit direct administrative logins.

### Step 6: Verify Passwordless Root Login

* Tested the secure configuration by attempting a direct root login from the client host:
```bash
ssh -i keypair root@3.89.180.178

```



---

## Troubleshooting & Challenges Encountered

### 1. Key File Path Resolution

* **Issue:** Initially attempted to look for generated SSH keys inside the default system `/root/.ssh/` directory on the local machine rather than the user's home directory.
* **Resolution:** Located the key pair correctly in the home directory (`~/keypair` and `~/keypair.pub`) where `ssh-keygen` originally saved them.

### 2. Strict Private Key Permissions (`Permissions 0644 for 'keypair' are too open`)

* **Issue:** SSH rejected the connection attempt because the local private key file permissions were too readable by other system users.
* **Resolution:** Secured the private key file locally by applying strict read-only permissions (`chmod 400 keypair`), allowing SSH authentication to proceed successfully.

### 3. Remote Root Authentication & Placeholder Confusion

* **Issue:** Attempting to SSH into root directly before configuring `authorized_keys` resulted in `Permission denied (publickey)`, and accidentally pasting placeholder comments instead of the real public key string blocked initial root key recognition.
* **Resolution:** Logged in via the default `ec2-user` first, replaced any placeholder text in `/root/.ssh/authorized_keys` with the actual contents of `~/keypair.pub`, and verified seamless passwordless root access from the local client.

---

## Verification & Results

* Successfully established an encrypted, passwordless SSH session directly as `root` using **`keypair`** on a `t3.micro` instance (`3.89.180.178`).

---

## Key Takeaways

* **Key-Based Authentication:** Utilizing cryptographic public-private key pairs (`keypair`) eliminates the security risks associated with standard password-based remote server authentication.
* **Client-Side File Management:** Private key files must reside on the connecting client machine with restricted permissions (`400`), while public keys reside within authorized key stores on the remote target server.

<img width="1214" height="398" alt="image" src="https://github.com/user-attachments/assets/6f187311-27fc-4034-9bb1-e57c2715f72e" />

<img width="852" height="317" alt="image" src="https://github.com/user-attachments/assets/1dc9dd1e-d135-41d5-a8e5-5512448324d9" />
