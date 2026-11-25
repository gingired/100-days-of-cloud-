# Day 38: Deploying Containerized Applications with Amazon ECS

## Overview

In this lab, I created a private Amazon ECR repository (`nautilus-ecr`), built and pushed a Docker image from `/root/pyapp` on `aws-client`, and deployed the containerized application onto a Fargate-backed Amazon ECS cluster (`nautilus-cluster`) via a task definition (`nautilus-taskdefinition`) and service (`nautilus-service`). After configuring HTTP access on the security group, I verified that the running Fargate task served the application over its public IP address.

---

## Lab Requirements

* **Private ECR Repository:** `nautilus-ecr`
* **Build Context & Tag:** `/root/pyapp` on `aws-client`, tagged as `latest` and pushed to `nautilus-ecr`
* **ECS Cluster:** `nautilus-cluster` (Fargate launch type)
* **ECS Task Definition:** `nautilus-taskdefinition` (using the `nautilus-ecr` image with configured CPU and memory resources)
* **ECS Service:** `nautilus-service` running on `nautilus-cluster` with at least `1` running task
* **Network Access:** Inbound `HTTP` rule (`0.0.0.0/0`) on the `default` security group for browser verification

---

## Step-by-Step Implementation

### Step 1: Create a Private ECR Repository (`nautilus-ecr`)

* Opened the **Amazon ECR Console**, selected **Private repositories**, and clicked **Create repository**.
* Entered **`nautilus-ecr`** as the repository name and selected **Create**.

### Step 2: Build, Tag, and Push the Docker Image

* Authenticated Docker to the target Amazon ECR registry:
```bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com

```


* Navigated to `/root/pyapp` on `aws-client`, built the image with the `latest` tag, tagged it with the ECR repository URI, and pushed it to `nautilus-ecr`:
```bash
cd /root/pyapp
docker build -t nautilus-ecr:latest .
docker tag nautilus-ecr:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/nautilus-ecr:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/nautilus-ecr:latest

```



### Step 3: Create and Configure the ECS Cluster (`nautilus-cluster`)

* Opened the **Amazon ECS Console**, selected **Clusters**, and clicked **Create**.
* Entered **`nautilus-cluster`** under **Cluster configuration**, selected **Fargate** infrastructure, and clicked **Create**.

### Step 4: Create the ECS Task Definition (`nautilus-taskdefinition`)

* Selected **Task definitions** from the ECS navigation pane and clicked **Create new task definition**.
* Entered **`nautilus-taskdefinition`**, selected **AWS Fargate**, configured the OS and task CPU/memory resources, and selected the image from `nautilus-ecr` via **Browse ECR images**.
* Kept remaining settings as default, clicked **Create**, and reviewed the created task definition.

### Step 5: Deploy the Application Using an ECS Service (`nautilus-service`)

* Opened **`nautilus-cluster`** under **Clusters**, went to the **Services** tab, and clicked **Create**.
* Selected `nautilus-taskdefinition`, entered **`nautilus-service`** as the service name, and chose **Fargate** as the launch type.
* Selected the default VPC and subnets, kept remaining options as default, and clicked **Create**.

---

## Verification & Results

* Verified that `nautilus-service` was **`Active`** with `1` running task under the **Service overview** tab.
* Added an inbound **HTTP** rule for `0.0.0.0/0` on the `default` security group in the VPC dashboard.
* Located the running task under `Clusters > nautilus-cluster > Services > nautilus-service > Tasks`, opened its public IP address in a browser, and confirmed `Welcome to KKE AWS cloud labs!` rendered on the page.

---

## Key Takeaways

* **Serverless Container Execution with Fargate:** Running ECS tasks on Fargate eliminates underlying EC2 host management while letting you define compute sizing and container images directly in the task definition.
* **Registry-to-Runtime Alignment:** Matching the Docker image tag and registry URI in ECR with the task definition and ensuring inbound HTTP access on the task's security group allows external traffic to reach the running container cleanly.
