
# AWS CLI Commands

## General
```bash
# Configure AWS CLI
aws configure
```

## EC2 (Elastic Compute Cloud)
```bash
# Describe EC2 instances
aws ec2 describe-instances

# Start an EC2 instance
aws ec2 start-instances --instance-ids <instance-id>

# Stop an EC2 instance
aws ec2 stop-instances --instance-ids <instance-id>

# Terminate an EC2 instance
aws ec2 terminate-instances --instance-ids <instance-id>
```

## S3 (Simple Storage Service)
```bash
# List S3 buckets
aws s3 ls

# Create a new S3 bucket
aws s3 mb s3://<bucket-name>

# Copy a file to an S3 bucket
aws s3 cp <file-path> s3://<bucket-name>/<file-name>

# Sync a local folder with an S3 bucket
aws s3 sync <local-folder> s3://<bucket-name>/
```

## IAM (Identity and Access Management)
```bash
# List IAM users
aws iam list-users

# Create a new IAM user
aws iam create-user --user-name <username>

# Attach a policy to a user
aws iam attach-user-policy --user-name <username> --policy-arn arn:aws:iam::aws:policy/<policy-name>
```

## Lambda
```bash
# List Lambda functions
aws lambda list-functions

# Invoke a Lambda function
aws lambda invoke --function-name <function-name> output.json
```

## RDS (Relational Database Service)
```bash
# Describe RDS instances
aws rds describe-db-instances

# Start an RDS instance
aws rds start-db-instance --db-instance-identifier <db-instance-id>

# Stop an RDS instance
aws rds stop-db-instance --db-instance-identifier <db-instance-id>
```
