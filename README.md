```yaml
# ================================================================
# AWS CLOUDFORMATION LAB
# EC2 + EBS + S3 INFRASTRUCTURE DEPLOYMENT
# ================================================================

project:
  name: ec2-ebs-s3-cloudformation-lab
  type: infrastructure-as-code
  platform: AWS
  provisioning_tool: CloudFormation

overview:
  description: >
    This project provisions AWS infrastructure using a CloudFormation template.
    It demonstrates EC2 deployment, EBS attachment, S3 provisioning,
    and basic networking/security configuration.

resources_created:

  security_group:
    type: AWS::EC2::SecurityGroup
    logical_name: InstanceSecurityGroup
    inbound_rules:
      - protocol: tcp
        port: 22
        source: 0.0.0.0/0   # ⚠ Open to world (lab only)

  ec2_instance:
    type: AWS::EC2::Instance
    logical_name: MyInstance
    instance_type: t2.micro
    ami: ami-0f3caa1cf4417e51b  # region dependent

  ebs_volume:
    type: AWS::EC2::Volume
    logical_name: MyVolume
    size_gib: 10
    availability_zone: same_as_instance

  volume_attachment:
    type: AWS::EC2::VolumeAttachment
    logical_name: MyVolumeAttachment
    device: /dev/sdf

  s3_bucket:
    type: AWS::S3::Bucket
    logical_name: MyS3Bucket
    bucket_name: my-unique-bucket-name-159753159753
    global_uniqueness_required: true

prerequisites:
  - aws_account_with_ec2_permissions
  - aws_account_with_s3_permissions
  - aws_cli_optional
  - key_pair_not_included

deployment:

  console:
    - navigate_to: CloudFormation
    - select: Create Stack
    - upload: template.yaml
    - wait_for: CREATE_COMPLETE

  cli:
    command: |
      aws cloudformation deploy \
        --template-file template.yaml \
        --stack-name ec2-instance-lab \
        --capabilities CAPABILITY_NAMED_IAM

security_considerations:
  ssh_access: 0.0.0.0/0
  recommendation: restrict_to_your_public_ip

cleanup:
  cli_command: |
    aws cloudformation delete-stack \
      --stack-name ec2-instance-lab

lessons_learned:
  - cloudformation_resource_dependencies
  - infrastructure_repeatability
  - ebs_attachment_via_iac
  - s3_global_naming_constraints
  - stack_lifecycle_management

future_improvements:
  - parameterize_instance_type
  - parameterize_ami
  - restrict_ssh_access
  - add_iam_role
  - enable_s3_versioning
  - add_cloudwatch_logging

author:
  name: Cole Greashaber
  focus: Cloud + Cybersecurity
  certifications:
    - AWS Certified Cloud Practitioner
    - CompTIA Security+
```
