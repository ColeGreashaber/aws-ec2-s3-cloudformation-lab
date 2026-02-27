#  AWS CloudFormation Lab  
## EC2 + EBS + S3 Infrastructure Deployment

![AWS](https://img.shields.io/badge/AWS-CloudFormation-orange)
![IaC](https://img.shields.io/badge/Infrastructure%20as%20Code-Yes-blue)
![Status](https://img.shields.io/badge/Project-Lab-green)

---

##  Overview

This project provisions infrastructure using **AWS CloudFormation**.

###  Resources Deployed

```yaml
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
  SecurityGroup:
    Type: AWS::EC2::SecurityGroup
  EBSVolume:
    Type: AWS::EC2::Volume
  S3Bucket:
    Type: AWS::S3::Bucket
```

-  EC2 Instance (`t2.micro`)
-  Security Group (SSH – TCP 22)
-  10 GiB EBS Volume
-  S3 Bucket (globally unique)

---

##  Architecture Breakdown

###  Security Group

```yaml
Type: AWS::EC2::SecurityGroup
Properties:
  SecurityGroupIngress:
    - IpProtocol: tcp
      FromPort: 22
      ToPort: 22
      CidrIp: 0.0.0.0/0
```

---

###  EC2 Instance

```yaml
Type: AWS::EC2::Instance
Properties:
  InstanceType: t2.micro
  ImageId: ami-0f3caa1cf4417e51b
```

---

###  EBS Volume

```yaml
Type: AWS::EC2::Volume
Properties:
  Size: 10
  AvailabilityZone: us-east-1a
```

---

###  S3 Bucket

```yaml
Type: AWS::S3::Bucket
Properties:
  BucketName: my-unique-bucket-name-159753159753
```

---

##  Deployment (CLI)

```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ec2-instance-lab \
  --capabilities CAPABILITY_NAMED_IAM
```

---

##  Security Considerations

```diff
- SSH open to 0.0.0.0/0 (Not recommended for production)
+ Restrict SSH to your public IP address
```

---

##  Key Takeaways

```bash
✔ Infrastructure as Code
✔ Resource dependencies
✔ EBS attachment via template
✔ S3 global naming constraints
✔ Stack lifecycle management
```

---

##  Cleanup

```bash
aws cloudformation delete-stack --stack-name ec2-instance-lab
```

---
