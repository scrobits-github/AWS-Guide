# AWS Fundamentals

This section introduces you to Amazon Web Services (AWS) and its core services. You'll learn how to set up an AWS account, navigate the AWS Management Console, and start using fundamental AWS services.

## Getting Started with AWS

### Creating an AWS Free Tier Account

1. **Sign up for AWS Free Tier**:
   - Visit [AWS Free Tier](https://aws.amazon.com/free/)
   - Click "Create a Free Account"
   - Follow the registration process
   - Provide a credit card (required but won't be charged unless you exceed free tier limits)
   - Complete the identity verification

2. **Free Tier Benefits**:
   - 12 months of limited free usage for many services
   - Some services have always-free tiers
   - Perfect for learning and small projects

3. **Setting Up Security**:
   - Enable Multi-Factor Authentication (MFA) for your root account
   - Create an IAM user for daily tasks
   - Set up billing alerts to avoid unexpected charges

### AWS Management Console

1. **Console Overview**:
   - Dashboard and service navigation
   - Region selector
   - Account and billing information
   - Support center

2. **AWS Regions and Availability Zones**:
   - Understanding global infrastructure
   - Choosing the right region for your applications
   - Region vs. Availability Zone vs. Edge Location

3. **AWS Support Plans**:
   - Basic (free)
   - Developer
   - Business
   - Enterprise

### AWS Command Line Interface (CLI)

1. **Installing AWS CLI**:
   ```bash
   # For macOS
   brew install awscli
   
   # For Windows (using PowerShell)
   msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
   
   # For Linux
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   ```

2. **Configuring AWS CLI**:
   ```bash
   aws configure
   ```
   You'll need to provide:
   - AWS Access Key ID
   - AWS Secret Access Key
   - Default region
   - Default output format

3. **Basic CLI Commands**:
   ```bash
   # List S3 buckets
   aws s3 ls
   
   # Describe EC2 instances
   aws ec2 describe-instances
   
   # Create an S3 bucket
   aws s3 mb s3://my-unique-bucket-name
   ```

## Core AWS Services

### Identity and Access Management (IAM)

1. **IAM Components**:
   - Users: Individual people or services
   - Groups: Collections of users
   - Roles: Sets of permissions for AWS services or external users
   - Policies: Documents defining permissions

2. **IAM Best Practices**:
   - Follow the principle of least privilege
   - Use groups to assign permissions
   - Enable MFA for all users
   - Regularly rotate credentials
   - Use IAM roles for applications on EC2

3. **Hands-on Exercise: Setting up IAM Users and Groups**:
   - Create an Admin group with AdministratorAccess policy
   - Create a user and add to the Admin group
   - Set up MFA for the user
   - Generate access keys for programmatic access

### Amazon EC2 (Elastic Compute Cloud)

1. **EC2 Basics**:
   - Virtual servers in the cloud
   - Various instance types optimized for different use cases
   - Amazon Machine Images (AMIs)
   - Instance purchasing options (On-Demand, Reserved, Spot)

2. **EC2 Instance Types**:
   - General Purpose (T3, M5)
   - Compute Optimized (C5)
   - Memory Optimized (R5)
   - Storage Optimized (I3, D2)
   - Accelerated Computing (P3, G4)

3. **Hands-on Exercise: Launching Your First EC2 Instance**:
   - Launch a t2.micro instance (free tier eligible)
   - Connect to your instance using SSH
   - Install a web server
   - Create a custom AMI

### Amazon S3 (Simple Storage Service)

1. **S3 Basics**:
   - Object storage service
   - Unlimited storage capacity
   - Highly durable and available
   - Use cases: backup, static website hosting, data lakes

2. **S3 Storage Classes**:
   - Standard
   - Intelligent-Tiering
   - Standard-IA (Infrequent Access)
   - One Zone-IA
   - Glacier
   - Glacier Deep Archive

3. **Hands-on Exercise: Creating an S3 Static Website**:
   - Create an S3 bucket
   - Upload HTML, CSS, and JavaScript files
   - Configure bucket for static website hosting
   - Set appropriate permissions

### Amazon RDS (Relational Database Service)

1. **RDS Basics**:
   - Managed relational database service
   - Supported engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora
   - Automated backups and patching
   - Multi-AZ deployment for high availability

2. **RDS vs. Self-Managed Databases**:
   - Advantages of managed service
   - When to choose RDS vs. EC2-hosted databases

3. **Hands-on Exercise: Creating an RDS Instance**:
   - Launch a MySQL RDS instance (free tier eligible)
   - Connect to the database using a client
   - Create tables and insert data
   - Create a database snapshot

### Amazon VPC (Virtual Private Cloud)

1. **VPC Basics**:
   - Isolated network in the AWS cloud
   - Subnets, route tables, and internet gateways
   - Security groups and network ACLs
   - VPC peering and VPN connections

2. **VPC Components**:
   - CIDR blocks and IP addressing
   - Public vs. private subnets
   - NAT gateways
   - Elastic IPs

3. **Hands-on Exercise: Creating a Custom VPC**:
   - Design a VPC with public and private subnets
   - Configure route tables and internet gateway
   - Launch EC2 instances in different subnets
   - Test connectivity

## Project: Deploy a Simple React App on AWS

In this project, you'll apply what you've learned to deploy a simple React application using AWS services.

### Step 1: Set Up the Environment

1. Create an IAM user with appropriate permissions
2. Configure AWS CLI with the user's credentials

### Step 2: Create an S3 Bucket for Static Website Hosting

```bash
# Create an S3 bucket
aws s3 mb s3://my-react-app-bucket-12345

# Enable static website hosting
aws s3 website s3://my-react-app-bucket-12345 --index-document index.html --error-document index.html

# Set bucket policy for public read access
cat > bucket-policy.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-react-app-bucket-12345/*"
    }
  ]
}
EOF

aws s3api put-bucket-policy --bucket my-react-app-bucket-12345 --policy file://bucket-policy.json
```

### Step 3: Build and Deploy the React App

1. Create a simple React app:
```bash
npx create-react-app my-react-app
cd my-react-app
```

2. Build the app:
```bash
npm run build
```

3. Deploy to S3:
```bash
aws s3 sync build/ s3://my-react-app-bucket-12345
```

### Step 4: Set Up CloudFront for Content Delivery

1. Create a CloudFront distribution pointing to your S3 bucket
2. Configure HTTPS and caching settings
3. Access your application via the CloudFront domain name

### Step 5: Set Up a Custom Domain (Optional)

1. Register a domain in Route 53 or use an existing domain
2. Create a hosted zone in Route 53
3. Create an A record pointing to your CloudFront distribution
4. Configure CloudFront to use your custom domain

## Next Steps

Now that you understand the fundamental AWS services, you're ready to explore more intermediate concepts. Continue to the [Intermediate AWS Concepts](../03-intermediate-aws/README.md) section to deepen your knowledge.

## Additional Resources

- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Training and Certification](https://aws.amazon.com/training/)