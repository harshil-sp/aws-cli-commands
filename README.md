# 🌩️ AWS CLI & EC2 Setup Guide 🌩️

This guide walks you through setting up AWS via CLI, configuring EC2, creating security groups, managing IAM users, and more! Let's get started 🚀.

---

## 1. ⚙️ Configuring AWS CLI
First, configure the AWS CLI with your credentials:
```bash
aws configure
```
This command will prompt you to enter your AWS Access Key ID, Secret Access Key, default region, and output format.

---

## 2. 🔒 Describing EC2 Security Groups
Security groups act as a virtual firewall for your EC2 instances, controlling inbound and outbound traffic. 

To describe the security groups:
```bash
aws ec2 describe-security-groups
```

---

## 3. 🌐 Describing EC2 VPC
VPC (Virtual Private Cloud) allows you to launch AWS resources into a virtual network. To describe VPCs:
```bash
aws ec2 describe-vpcs
```

---

## 4. 🛡️ Creating a Security Group
Create a security group to manage your EC2 instance's traffic:
```bash
aws ec2 create-security-group --group-name my-sec-group --description "My security group"
```

---

## 5. 🔎 Describing Specific EC2 Security Groups
To describe a specific security group:
```bash
aws ec2 describe-security-groups --group-ids <your-group-id>
```

---

## 6. 🔑 Adding an Inbound Rule for SSH
Allow SSH access by adding an inbound rule to the security group:
```bash
aws ec2 authorize-security-group-ingress --group-id <your-group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

## 7. 🗝️ Creating a Key Pair
Create a key pair and download it locally:
```bash
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```
The `--query` parameter allows you to filter the output to only include the required information, in this case, the key material.

---

## 8. 🛣️ Describe Subnets
Subnets divide your VPC and allow you to control network traffic:
```bash
aws ec2 describe-subnets
```

---

## 9. 🔍 Finding Amazon Linux AMI ID
To find the Amazon Machine Image (AMI) for Amazon Linux:
```bash
aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-*" --query 'Images[0].ImageId'
```

---

## 10. 🚀 Creating an EC2 Instance
Launch an EC2 instance:
```bash
aws ec2 run-instances --image-id <ami-id> --count 1 --instance-type t2.micro --key-name MyKeyPair
```

---

## 11. 🖥️ Describing Created EC2 Instances
Describe the EC2 instances you’ve launched:
```bash
aws ec2 describe-instances
```

---

## 12. 🛠️ Filters and Query in AWS Command
- `--filter`: Allows you to limit the results to a specific condition.
- `--query`: Allows you to project only a subset of the results.

Example:
```bash
aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query 'Reservations[*].Instances[*].{Instance:InstanceId,State:State.Name}'
```

---

## 13. ⚡ Filtering EC2 Instances with t2.micro
List EC2 instances of type `t2.micro` with only basic details:
```bash
aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query "Reservations[*].Instances[*].{InstanceID:InstanceId,State:State.Name}"
```

---

## 14. 👥 Creating an IAM User Group
Create a group for your IAM users:
```bash
aws iam create-group --group-name my-group
```

---

## 15. 🆔 AWS ARN Purpose
An Amazon Resource Name (ARN) uniquely identifies AWS resources. For example:
```bash
arn:aws:iam::123456789012:group/my-group
```

---

## 16. 🙋 Creating an IAM User
Create a new IAM user:
```bash
aws iam create-user --user-name new-user
```

---

## 17. 👥 Adding IAM User to Group
Add the newly created user to the group:
```bash
aws iam add-user-to-group --user-name new-user --group-name my-group
```

---

## 18. 👤 Describing Group
Describe the IAM group you’ve created:
```bash
aws iam get-group --group-name my-group
```

---

## 19. 📜 Attaching Policy to Group
Grant EC2 full access to the group:
```bash
aws iam attach-group-policy --group-name my-group --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
```

---

## 20. 📜 Attaching Policy to User
Attach the EC2 full access policy to a user:
```bash
aws iam attach-user-policy --user-name new-user --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
```

---

## 21. 🔍 Listing IAM Policies with --query
List IAM policies with the `--query` parameter and various output formats:
```bash
aws iam list-policies --query 'Policies[*].{PolicyName:PolicyName,ARN:Arn}' --output table
```

---

## 22. 📋 Listing Attached Policies for a Group
To list the policies attached to a specific group:
```bash
aws iam list-attached-group-policies --group-name my-group
```

---

## 23. 🔑 Creating a Login Profile for a User
Create a login profile for the user with password reset required:
```bash
aws iam create-login-profile --user-name new-user --password 'MySecurePassword123' --password-reset-required
```

---

## 24. 🔐 Creating IAM:ChangePassword Policy
Create a policy allowing users to change their password:
```bash
aws iam create-policy --policy-name ChangePasswordPolicy --policy-document file://change_password_policy.json
```
Attach this policy to the user:
```bash
aws iam attach-user-policy --user-name new-user --policy-arn arn:aws:iam::aws:policy/ChangePasswordPolicy
```

---

## 25. 📛 Creating IAM Access for New User
To create access keys for the new IAM user:
```bash
aws iam create-access-key --user-name new-user
```

---
