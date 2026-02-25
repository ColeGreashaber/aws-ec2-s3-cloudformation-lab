# AWS CloudFormation Lab: EC2 + EBS + S3

## Overview
This project provisions a small AWS environment using **AWS CloudFormation**. The template deploys:
- An **EC2 instance** (t2.micro)
- A **Security Group** allowing **SSH (port 22)**
- A **10 GiB EBS volume** and attachment to the instance
- An **S3 bucket** with a custom bucket name

This lab is designed to build hands-on experience with Infrastructure as Code (IaC), basic networking/security configuration, and common AWS resource relationships.

---

## Architecture
**Resources created:**
- **AWS::EC2::SecurityGroup** (`InstanceSecurityGroup`)
  - Inbound rule: TCP 22 from `0.0.0.0/0` (SSH)
- **AWS::EC2::Instance** (`MyInstance`)
  - AMI: `ami-0f3caa1cf4417e51b` (region-dependent)
  - Instance type: `t2.micro`
- **AWS::EC2::Volume** (`MyVolume`)
  - Size: 10 GiB
  - AZ: same Availability Zone as the instance
- **AWS::EC2::VolumeAttachment** (`MyVolumeAttachment`)
  - Attaches volume to instance at `/dev/sdf`
- **AWS::S3::Bucket** (`MyS3Bucket`)
  - Bucket name: `my-unique-bucket-name-159753159753` (must be globally unique)

---

## Prerequisites
- An AWS account with permissions to create EC2, EBS, Security Groups, and S3 resources
- AWS CLI installed (optional, if deploying via CLI)
- A key pair (optional, **not included** in this template)

> **Note:** This template currently opens SSH to the world (`0.0.0.0/0`). For real environments, restrict this to your IP range.

---

## Deploying the Stack

### Option A: AWS Console (CloudFormation)
1. Go to **CloudFormation** → **Create stack**
2. Choose **Upload a template file** and select `template.yaml`
3. Follow the prompts and create the stack
4. Monitor progress until status shows **CREATE_COMPLETE**

### Option B: AWS CLI
From the project directory:
```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ec2-instance-lab \
  --capabilities CAPABILITY_NAMED_IAM