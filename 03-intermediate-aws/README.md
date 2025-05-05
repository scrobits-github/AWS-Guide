# Intermediate AWS Concepts

This section builds upon the AWS fundamentals and introduces more advanced concepts and services. You'll learn how to architect scalable, highly available applications using AWS services.

## Advanced EC2 Concepts

### Instance Types and Sizing

1. **Choosing the Right Instance Type**:
   - Matching instance types to workloads
   - Understanding CPU credits for burstable instances
   - Memory-to-vCPU ratio considerations

2. **EC2 Instance Lifecycle**:
   - Launch, stop, start, terminate
   - Hibernate option
   - Instance store vs. EBS-backed instances

3. **Hands-on Exercise: Instance Type Comparison**:
   - Launch different instance types
   - Run benchmark tests
   - Compare performance metrics
   - Analyze cost-performance tradeoffs

### Amazon EC2 Auto Scaling

1. **Auto Scaling Components**:
   - Launch templates/configurations
   - Auto Scaling groups
   - Scaling policies

2. **Scaling Strategies**:
   - Manual scaling
   - Scheduled scaling
   - Dynamic scaling (target tracking, step scaling, simple scaling)
   - Predictive scaling

3. **Hands-on Exercise: Setting Up Auto Scaling**:
   - Create a launch template
   - Configure an Auto Scaling group
   - Set up dynamic scaling policies
   - Test scaling by generating load

### Elastic Load Balancing (ELB)

1. **Types of Load Balancers**:
   - Application Load Balancer (ALB)
   - Network Load Balancer (NLB)
   - Gateway Load Balancer (GWLB)
   - Classic Load Balancer (CLB) - legacy

2. **Load Balancer Features**:
   - Health checks
   - SSL/TLS termination
   - Sticky sessions
   - Path-based routing (ALB)
   - Host-based routing (ALB)

3. **Hands-on Exercise: Implementing Load Balancing**:
   - Create an Application Load Balancer
   - Register EC2 instances as targets
   - Configure health checks
   - Test load distribution

### Building Highly Available Architectures

1. **Multi-AZ Deployments**:
   - Designing for availability zone failures
   - Active-passive vs. active-active configurations

2. **Fault Tolerance Strategies**:
   - Redundancy
   - Graceful degradation
   - Retry mechanisms
   - Circuit breakers

3. **Hands-on Exercise: Creating a Highly Available Web Application**:
   - Deploy EC2 instances across multiple AZs
   - Set up an ALB with cross-zone load balancing
   - Configure Auto Scaling for resilience
   - Test failover scenarios

## Advanced Storage Solutions

### Amazon EBS (Elastic Block Store)

1. **EBS Volume Types**:
   - General Purpose SSD (gp2/gp3)
   - Provisioned IOPS SSD (io1/io2)
   - Throughput Optimized HDD (st1)
   - Cold HDD (sc1)

2. **EBS Features**:
   - Snapshots and AMIs
   - Encryption
   - Multi-attach (for io1/io2)
   - Fast snapshot restore

3. **Hands-on Exercise: EBS Performance Optimization**:
   - Create different types of EBS volumes
   - Benchmark I/O performance
   - Create and restore from snapshots
   - Implement EBS encryption

### Amazon S3 Advanced Features

1. **S3 Storage Classes Deep Dive**:
   - Performance characteristics
   - Durability and availability guarantees
   - Minimum storage duration
   - Retrieval fees

2. **S3 Data Management**:
   - Lifecycle policies
   - Replication (cross-region and same-region)
   - Object versioning
   - Object lock

3. **S3 Security**:
   - Bucket policies
   - Access control lists (ACLs)
   - Block public access settings
   - S3 access points

4. **Hands-on Exercise: Implementing S3 Best Practices**:
   - Configure lifecycle policies
   - Set up cross-region replication
   - Implement versioning
   - Secure buckets with appropriate policies

### Amazon EFS (Elastic File System)

1. **EFS Basics**:
   - Fully managed NFS file system
   - Elastic scaling
   - Multi-AZ architecture
   - Use cases: content management, web serving, data sharing

2. **EFS Performance Modes**:
   - General Purpose
   - Max I/O

3. **EFS Throughput Modes**:
   - Bursting
   - Provisioned

4. **Hands-on Exercise: Setting Up EFS**:
   - Create an EFS file system
   - Mount EFS on multiple EC2 instances
   - Test concurrent access
   - Benchmark performance

## Networking and Content Delivery

### Advanced VPC Concepts

1. **VPC Endpoints**:
   - Gateway endpoints
   - Interface endpoints (powered by AWS PrivateLink)
   - Services supported by VPC endpoints

2. **Transit Gateway**:
   - Centralized hub for VPC and on-premises networks
   - Transitive peering
   - Route tables and attachments

3. **Hands-on Exercise: VPC Advanced Networking**:
   - Set up VPC endpoints for S3 and DynamoDB
   - Configure interface endpoints for AWS services
   - Test private access to AWS services

### Amazon CloudFront

1. **CloudFront Concepts**:
   - Distributions
   - Origins and origin groups
   - Cache behaviors
   - Edge locations and regional edge caches

2. **CloudFront Features**:
   - HTTPS and custom SSL certificates
   - Origin failover
   - Field-level encryption
   - Lambda@Edge and CloudFront Functions

3. **Hands-on Exercise: Optimizing Content Delivery with CloudFront**:
   - Create a CloudFront distribution with S3 origin
   - Configure cache behaviors
   - Set up custom error responses
   - Analyze cache statistics

### Route 53 Advanced Routing

1. **DNS Record Types**:
   - A, AAAA, CNAME, MX, TXT, NS, SOA
   - Alias records vs. CNAME records

2. **Routing Policies**:
   - Simple routing
   - Weighted routing
   - Latency-based routing
   - Geolocation routing
   - Geoproximity routing
   - Failover routing
   - Multivalue answer routing

3. **Hands-on Exercise: Implementing Advanced DNS Strategies**:
   - Set up health checks
   - Configure failover routing
   - Implement latency-based routing
   - Test global traffic distribution

## Database Services

### Amazon RDS Advanced Features

1. **Multi-AZ Deployments**:
   - Synchronous replication
   - Automatic failover
   - Increased availability

2. **Read Replicas**:
   - Asynchronous replication
   - Scaling read operations
   - Cross-region read replicas

3. **Performance Insights**:
   - Database load monitoring
   - Identifying performance bottlenecks
   - Query analysis

4. **Hands-on Exercise: RDS High Availability Setup**:
   - Create a Multi-AZ RDS instance
   - Set up read replicas
   - Test failover scenarios
   - Monitor performance with Performance Insights

### Amazon Aurora

1. **Aurora Architecture**:
   - Distributed, fault-tolerant storage
   - 6-way replication
   - Self-healing storage

2. **Aurora Features**:
   - Aurora Serverless
   - Global Database
   - Backtrack
   - Parallel Query

3. **Hands-on Exercise: Migrating to Aurora**:
   - Create an Aurora cluster
   - Migrate data from RDS
   - Test performance differences
   - Configure Aurora Serverless

### Amazon DynamoDB

1. **DynamoDB Basics**:
   - NoSQL key-value and document database
   - Tables, items, and attributes
   - Primary keys (partition key and sort key)

2. **DynamoDB Capacity Modes**:
   - Provisioned capacity
   - On-demand capacity

3. **Advanced DynamoDB Features**:
   - Global tables
   - DAX (DynamoDB Accelerator)
   - Time to Live (TTL)
   - Streams

4. **Hands-on Exercise: Building with DynamoDB**:
   - Create a DynamoDB table
   - Implement basic CRUD operations
   - Use secondary indexes
   - Set up DynamoDB Streams

## Project: Scalable Web Application Architecture

In this project, you'll apply intermediate AWS concepts to build a scalable web application architecture.

### Architecture Overview

- Multi-AZ deployment with Auto Scaling
- Application Load Balancer for traffic distribution
- Amazon RDS with read replicas for database
- ElastiCache for session management
- S3 and CloudFront for static content delivery

### Step 1: Set Up the Network Infrastructure

1. Create a VPC with public and private subnets across multiple AZs
2. Configure route tables, internet gateway, and NAT gateways
3. Set up security groups and network ACLs

### Step 2: Deploy the Database Tier

1. Create an RDS instance in Multi-AZ configuration
2. Set up read replicas for read scaling
3. Configure parameter groups and option groups

### Step 3: Implement Caching

1. Create an ElastiCache cluster
2. Configure security groups for cache access
3. Set up monitoring and alarms

### Step 4: Deploy the Application Tier

1. Create a launch template for EC2 instances
2. Set up an Auto Scaling group across multiple AZs
3. Configure scaling policies based on CPU utilization

### Step 5: Set Up the Web Tier

1. Create an Application Load Balancer
2. Configure target groups and health checks
3. Set up SSL/TLS termination

### Step 6: Implement Content Delivery

1. Create an S3 bucket for static assets
2. Set up a CloudFront distribution
3. Configure cache behaviors and origin settings

### Step 7: Test and Optimize

1. Test failover scenarios
2. Simulate traffic spikes to verify Auto Scaling
3. Optimize performance and cost

## Next Steps

Now that you've mastered intermediate AWS concepts, you're ready to explore advanced AWS services. Continue to the [Advanced AWS Services](../04-advanced-aws/README.md) section to further enhance your AWS skills.

## Additional Resources

- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Solutions Library](https://aws.amazon.com/solutions/)
- [AWS Whitepapers](https://aws.amazon.com/whitepapers/)