# cloudformation


# AWS CloudFormation Templates

This repository contains CloudFormation templates for provisioning EC2-based infrastructure in a scalable and highly available way using Auto Scaling Groups (ASG) and Elastic Load Balancer (ELB).

---

## 📁 Templates Overview

### 1. `ec2_cloudformation.yml` – Basic EC2 Instance

This template launches a standalone EC2 instance with the following features:

- Launches a single EC2 instance in a specified subnet
- Installs Apache HTTP server
- Optionally allows SSH and HTTP access via security group

> ✅ Good for: Basic testing, quick deployment scenarios

---

### 2. `ec2_cloudformation_asg.yml` – EC2 with Auto Scaling Group

This template sets up:

- A Launch Template for EC2 instances
- An Auto Scaling Group (ASG) with configurable min/max/desired instances
- Apache installed and served on boot
- Security group allowing SSH and HTTP access
- EBS volume mounted to `/app`

> ✅ Good for: High availability and scalable backend services

---

### 3. `ec2_cloudformation_asg_elb.yml` – EC2 with ASG and Application Load Balancer (ALB)

This template includes everything from the previous ASG template, and additionally:

- An internet-facing Application Load Balancer (ALB)
- Listener forwarding traffic to EC2 instances on port 80
- Two subnets across different Availability Zones (required for ALB)
- Health checks configured for auto-scaling
- Mounted EBS volume for each instance

> ✅ Good for: Production-grade, fault-tolerant, scalable web applications

---

## 🛠 How to Deploy

```bash
# Replace parameters with actual values

aws cloudformation create-stack \
  --stack-name MyEC2Stack \
  --template-body file://ec2_cloudformation_asg_elb.yml \
  --parameters \
    ParameterKey=KeyName,ParameterValue=YourKeyPairName \
    ParameterKey=AmiId,ParameterValue=ami-0abc123456789def0 \
    ParameterKey=Subnet1,ParameterValue=subnet-aaaa1111 \
    ParameterKey=Subnet2,ParameterValue=subnet-bbbb2222 \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-east-2
++++++++++++++++++++++++++++++++++++++++++++++++
AWS Cloudformation template
Use full commands 
aws cloudformation create-stack \
  --stack-name MyEC2Stack \
  --template-body file://ec2_cloudformation_asg.yml \
  --parameters \
    ParameterKey=KeyName,ParameterValue=yourkey_name \
    ParameterKey=AmiId,ParameterValue=ami-0c803b171269e2d72 \
    ParameterKey=Subnet1,ParameterValue=subnet-0e5e7ebd8ffbdc624\
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-east-2

aws cloudformation create-stack \
  --stack-name MyEC2Stack \
  --template-body file://ec2_cloudformation_asg_elb.yml \
  --parameters \
    ParameterKey=KeyName,ParameterValue=yourkey_name \
    ParameterKey=AmiId,ParameterValue=ami-0c803b171269e2d72 \
    ParameterKey=Subnet1,ParameterValue=subnet-0e5e7ebd8ffbdc624 \
    ParameterKey=Subnet2,ParameterValue=subnet-01c5b14b1c2d3bc25 \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-east-2

aws cloudformation update-stack \
--stack-name MyEC2Stack \
--template-body file://ec2_cloudformation.yml \
--parameters \
ParameterKey=KeyName,ParameterValue=yourkey_name \
ParameterKey=AmiId,ParameterValue=ami-0c803b171269e2d72

aws cloudformation delete-stack --stack-name MyEC2Stack

aws cloudformation describe-stacks --stack-name MyEC2Stack


