# AWS CodeDeploy - Hello World Application

This repository demonstrates how to set up AWS CodeDeploy to deploy a simple "Hello, World!" web page to an EC2 instance. The process involves creating IAM roles, configuring EC2 instances, setting up CodeDeploy, and deploying the application using GitHub and AWS CodePipeline.

## Prerequisites

Before you begin, make sure you have the following:

- AWS account with necessary permissions.
- GitHub account and repository.
- AWS CLI installed and configured on your local machine.
- EC2 instance set up and IAM roles created (as outlined in the setup steps below).

## Setup Instructions

### 1. **Creating IAM Roles**

- **EC2 Role with Permissions**
  1. Navigate to IAM > Roles > Create Role in the AWS Management Console.
  2. Choose EC2 as the trusted entity.
  3. Attach the `AWSCODEDEP` policy.
  4. Name the role `EC2RoleForAWSCODEDEP` and click "Create role".

- **CodeDeploy Role**
  1. Navigate to IAM > Roles > Create Role.
  2. Choose CodeDeploy as the trusted entity.
  3. Attach the `AWSCODEDEPLOY` policy.
  4. Name the role `CodeDeployRole` and click "Create role".

### 2. **Launching an EC2 Instance**

1. Go to EC2 > Launch Instance in the AWS Management Console.
2. Select Amazon Linux 2 AMI.
3. Choose an instance type (e.g., t2.micro for testing).
4. Under IAM role, select the `EC2RoleForAWSCODEDEP` role created earlier.
5. Add storage, tags, and configure security group (allow ports 22 and 80).
6. Click "Review and Launch", then "Launch".

### 3. **Add User Data for Dependencies Installation**

Add the following script in the **User Data** section during instance creation:

```bash
#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto
sudo yum install -y python-pip
sudo pip install awscli
