Day 28: Containerizing and Deploying a Python Application to Amazon ECR via EC2

Overview

In this lab, I containerized a Python application on an AWS EC2 Ubuntu instance and securely pushed the resulting Docker image to a private Amazon Elastic Container Registry (ECR) repository named gingired-ecr using an IAM role for authentication instead of static credentials.

Lab Requirements

Cloud Provider: Amazon Web Services (AWS)

Region: us-east-1

AWS Account ID: 523109312778

Compute Instance: AWS EC2 Ubuntu (i-0def00a1c3c99f217, Public IP: 3.87.96.1)

Authentication Method: IAM Role (iamrole_gingired) with ECR permissions

Target ECR Repository: gingired-ecr

Step-by-Step Implementation

Step 1: Establish SSH Connection to the EC2 Instance

Generated a secure SSH key pair (keypair_fresh) locally.

Registered the public key inside the remote instance's ~/.ssh/authorized_keys file with strict permissions (chmod 700 ~/.ssh and chmod 600 ~/.ssh/authorized_keys).

Connected successfully to the remote server via SSH: ssh -i ~/keypair_fresh ubuntu@3.87.96.1.

Step 2: Set Up the Application Directory and Dockerfile

Switched to the root user (sudo -i) and created the application workspace directory (/root/pyapp).

Created a lightweight Python container configuration (Dockerfile) utilizing python:3.9-slim.

Step 3: Install and Configure Docker

Updated the package lists and installed standard Docker (apt update && apt install -y docker.io).

Started and enabled the Docker daemon service (systemctl start docker).

Built the local container image using docker build -t python-image ..

Step 4: Configure AWS IAM Permissions and Credential Hygiene

Attached the required ECR policy (AmazonEC2ContainerRegistryPowerUser) to the EC2 instance's assigned IAM role (iamrole_gingired) via the AWS IAM Console.

Cleared out legacy local configuration files (rm -rf ~/.aws) to prevent credential conflicts and ensure the instance defaulted properly to its IAM role.

Verified proper role assumption by running aws sts get-caller-identity.

Step 5: Tag, Authenticate, and Push to Amazon ECR

Authenticated the Docker client with the private Amazon ECR registry using the AWS CLI login command.

Tagged the local build with the exact private ECR repository URI: docker tag python-image:latest 523109312778.dkr.ecr.us-east-1.amazonaws.com/gingired-ecr:latest.

Pushed the finalized image layers up to ECR using docker push.

Troubleshooting & Challenges Encountered

1. Missing Utilities on Fresh Instance (docker, aws)

Issue: Commands like docker and aws were not found on the fresh Ubuntu AMI when first running the containerization steps.

Resolution: Installed Docker via apt install docker.io and installed the official AWS CLI package using curl and unzip.

2. NoCredentials and Partial Credential Errors

Issue: Running aws configure created partial files in ~/.aws that conflicted with and overrode the instance's IAM role.

Resolution: Cleared out the local .aws configuration directory (rm -rf ~/.aws) so the AWS CLI automatically fell back to the instance's attached IAM role.

3. 403 Forbidden / Authorization Mismatch

Issue: Received a 403 Forbidden error during docker push due to a mismatch between the AWS Account ID in the registry URI and the instance's actual account ID.

Resolution: Updated the tag and push commands to precisely match the correct AWS Account ID (523109312778) and target repository (gingired-ecr).

4. AccessDeniedException on Repository Creation

Issue: The EC2 IAM role lacked ecr:CreateRepository permissions when attempting to create the repository via CLI.

Resolution: Pre-created the private repository (gingired-ecr) directly through the AWS Web Console.

Verification & Results

Verified that the Docker daemon is active and successfully builds local container images.

Confirmed that the EC2 instance successfully authenticates with ECR using its attached IAM role (iamrole_gingired).

Confirmed that image layers successfully upload and the latest tag appears under the gingired-ecr repository inside the Amazon ECR Console.

Key Takeaways

IAM Role Security: Attaching an IAM role directly to an EC2 instance provides secure, credential-free access to AWS services, eliminating the need for hardcoded access keys.

Registry URI Precision: Docker tags must precisely match the correct AWS Account ID, region, and ECR repository name to prevent 403 Forbidden authorization failures.

Credential Precedence: Stale or incomplete local configuration files in ~/.aws can override instance-level IAM roles, making clean environment hygiene essential.
