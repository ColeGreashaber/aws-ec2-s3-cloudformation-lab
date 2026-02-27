# 🚀 AWS CloudFormation Lab: EC2 + EBS + S3

## 📌 Overview

This project provisions AWS infrastructure using **AWS CloudFormation (Infrastructure as Code)**.

The template deploys:

-  **EC2 Instance** (`t2.micro`)
-  **Security Group** allowing SSH (TCP 22)
-  **10 GiB EBS Volume** attached to the instance
- **S3 Bucket** with a globally unique name

This lab demonstrates hands-on experience with:

- Infrastructure as Code (IaC)
- AWS resource dependencies
- Basic networking and security configuration
- Block and object storage provisioning
- Stack lifecycle management

---

##  Architecture

### Resources Created

**Security Group**  
- Type: `AWS::EC2::SecurityGroup`  
- Inbound rule: TCP 22 from `0.0.0.0/0`

**EC2 Instance**  
- Type: `AWS::EC2::Instance`  
- AMI: `ami-0f3caa1cf4417e51b` (region dependent)  
- Instance Type: `t2.micro`

**EBS Volume**  
- Type: `AWS::EC2::Volume`  
- Size: 10 GiB  
- Attached via `AWS::EC2::VolumeAttachment`  
- Device: `/dev/sdf`

**S3 Bucket**  
- Type: `AWS::S3::Bucket`  
- Bucket name must be globally unique  

---

##  Prerequisites

- AWS account with permissions for:
  - EC2
  - EBS
  - Security Groups
  - S3
- AWS CLI (optional)
- EC2 Key Pair (not included in template)

>  **Security Note:**  
> SSH is currently open to `0.0.0.0/0` for lab purposes.  
> In production, restrict access to your public IP range.

---

##  Deployment

### Option 1 – AWS Console

1. Navigate to **CloudFormation**
2. Select **Create Stack**
3. Upload `template.yaml`
4. Deploy and wait for `CREATE_COMPLETE`

---

### Option 2 – AWS CLI

```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ec2-instance-lab \
  --capabilities CAPABILITY_NAMED_IAM
```

---

##  Key Takeaways

- CloudFormation automatically manages resource dependencies  
- EBS volumes can be attached declaratively via IaC  
- Security group exposure should be minimized in production  
- S3 bucket names must be globally unique  
- Infrastructure can be repeatedly deployed and destroyed using stack management  

---

##  Cleanup

To avoid unnecessary AWS charges:

```bash
aws cloudformation delete-stack --stack-name ec2-instance-lab
```

Or delete the stack via the AWS Console.

---

##  Author

**Cole Greashaber**  
MIS – Cybersecurity  
AWS Certified Cloud Practitioner  
CompTIA Security+
