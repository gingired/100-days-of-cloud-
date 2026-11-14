
Day 28: Containerizing and Deploying a Python App to Amazon ECR 

Comprehensive documentation and implementation guide for containerizing a Python application on an AWS EC2 instance and securely pushing the image to a private Amazon Elastic Container Registry (ECR).

Step-by-Step Implementation Workflow

Step 1: Establish SSH Connection to the EC2 Instance

Generate a secure SSH key pair locally and register the public key on the remote instance:

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
ssh -i ~/keypair_fresh ubuntu@3.87.96.1


Step 2: Set Up the Application Directory and Dockerfile

Switch to root and create the application workspace with a lightweight Python container configuration:

sudo -i
mkdir -p /root/pyapp
cd /root/pyapp

cat << 'EOF' > Dockerfile
FROM python:3.9-slim
WORKDIR /app
CMD ["python", "-c", "print('Hello ECR')"]
EOF


Step 3: Install and Configure Docker

Update package listings, install standard Docker, and start the daemon:

apt update && apt install -y docker.io
systemctl start docker
systemctl enable docker

# Build local container image
docker build -t python-image .


Step 4: Configure AWS IAM Permissions & Clear Stale Credentials

Attach the AmazonEC2ContainerRegistryPowerUser or FullAccess policy to your EC2 IAM role (iamrole_gingired) via the AWS Console.

Clear out legacy local configuration files to prevent credential conflicts:

rm -rf ~/.aws


Verify proper role assumption:

aws sts get-caller-identity


Step 5: Tag, Authenticate, and Push to Amazon ECR

Authenticate Docker with your private ECR registry, tag your local build, and push the image:

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 523109312778.dkr.ecr.us-east-1.amazonaws.com

docker tag python-image:latest 523109312778.dkr.ecr.us-east-1.amazonaws.com/gingired-ecr:latest

docker push 523109312778.dkr.ecr.us-east-1.amazonaws.com/gingired-ecr:latest


Troubleshooting & Key Learnings

403 Forbidden / Authorization Errors: Ensure that the AWS Account ID in your Docker tag and login commands exactly matches your active AWS account (523109312778).

IAM Permissions: Running aws ecr create-repository requires ecr:CreateRepository permissions. If restricted, pre-create the private repository via the AWS Web Console.

Credential Priority: Clearing out stale local credentials (rm -rf ~/.aws) guarantees the EC2 instance correctly defaults to its attached IAM Role for authentication.
<img width="849" height="145" alt="image" src="https://github.com/user-attachments/assets/a616a266-b288-4c48-9c49-89859b504931" />
<img width="1170" height="439" alt="image" src="https://github.com/user-attachments/assets/d134810b-00c0-459f-ad5b-9772eacb2d55" />
