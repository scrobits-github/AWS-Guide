# Production-Grade Applications on AWS

This section focuses on building production-ready applications on AWS. You'll learn how to design, deploy, and maintain applications that meet enterprise requirements for reliability, scalability, security, and cost optimization.

## Building Production-Ready Applications

### Application Architecture Patterns

1. **Microservices Architecture**:
   - Service decomposition strategies
   - Inter-service communication
   - API design
   - Service discovery

2. **Event-Driven Architecture**:
   - Event sourcing
   - CQRS (Command Query Responsibility Segregation)
   - Pub/sub patterns
   - Event-driven microservices

3. **Serverless Architecture**:
   - Function composition
   - State management
   - Cold start mitigation
   - Cost optimization

4. **Hands-on Exercise: Architecture Decision Records**:
   - Document architecture decisions for a sample application
   - Evaluate tradeoffs between different patterns
   - Create architecture diagrams using AWS Architecture Icons

### Resilience Engineering

1. **Fault Tolerance Patterns**:
   - Circuit breaker
   - Bulkhead
   - Retry with exponential backoff
   - Fallback mechanisms

2. **Chaos Engineering**:
   - Principles of chaos engineering
   - Controlled experiments
   - AWS Fault Injection Simulator
   - Gamedays

3. **Disaster Recovery Strategies**:
   - Backup and restore
   - Pilot light
   - Warm standby
   - Multi-site active/active

4. **Hands-on Exercise: Building Resilient Systems**:
   - Implement circuit breaker pattern
   - Create chaos experiments
   - Design and test a disaster recovery plan

### Performance Optimization

1. **Application Performance Monitoring**:
   - CloudWatch Application Insights
   - X-Ray tracing
   - Custom metrics
   - Dashboards and alarms

2. **Database Performance**:
   - Query optimization
   - Connection pooling
   - Read replicas and sharding
   - Caching strategies

3. **Front-end Performance**:
   - Content delivery optimization
   - Client-side caching
   - Progressive web apps
   - Performance budgets

4. **Hands-on Exercise: Performance Testing and Optimization**:
   - Set up load testing with Artillery or JMeter
   - Identify and resolve performance bottlenecks
   - Implement caching at multiple layers
   - Optimize front-end delivery

### Cost Optimization

1. **AWS Cost Management Tools**:
   - AWS Cost Explorer
   - AWS Budgets
   - AWS Cost and Usage Reports
   - Trusted Advisor

2. **Cost Optimization Strategies**:
   - Right-sizing resources
   - Spot instances and Savings Plans
   - Serverless to reduce idle capacity
   - Storage tiering

3. **FinOps Practices**:
   - Tagging strategies
   - Cost allocation
   - Showback/chargeback models
   - Continuous cost optimization

4. **Hands-on Exercise: Implementing Cost Controls**:
   - Set up detailed cost allocation tags
   - Create budgets and alerts
   - Identify and eliminate waste
   - Implement automated scaling based on demand

## Real-World Application Examples

### React App on S3 and CloudFront

This example demonstrates how to deploy a production-grade React single-page application (SPA) using AWS services.

#### Architecture Components:
- Amazon S3 for hosting the React build files
- CloudFront for content delivery and HTTPS
- Route 53 for domain management
- AWS Certificate Manager for SSL/TLS certificates
- AWS WAF for security
- Amazon Cognito for authentication (optional)

#### Deployment Steps:

1. **Set Up the Infrastructure**:

```bash
# Create an S3 bucket for hosting
aws s3 mb s3://my-production-react-app

# Enable static website hosting
aws s3 website s3://my-production-react-app --index-document index.html --error-document index.html

# Create a CloudFront distribution
# (This would typically be done through the AWS Console or CloudFormation)
```

2. **Configure CI/CD Pipeline**:

```yaml
# Example GitHub Actions workflow
name: Deploy React App

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build
        
      - name: Deploy to S3
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --delete
        env:
          AWS_S3_BUCKET: my-production-react-app
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          SOURCE_DIR: 'build'
          
      - name: Invalidate CloudFront cache
        uses: chetan/invalidate-cloudfront-action@master
        env:
          DISTRIBUTION: ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }}
          PATHS: '/*'
          AWS_REGION: 'us-east-1'
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

3. **Implement Security Best Practices**:

- Configure bucket policy to allow CloudFront access only
- Set up WAF rules to protect against common web vulnerabilities
- Implement Content Security Policy headers
- Enable CloudFront access logs

4. **Optimize for Performance**:

- Configure appropriate cache behaviors in CloudFront
- Implement code splitting in React
- Use compression for assets
- Set up error monitoring with services like Sentry

5. **Monitor and Maintain**:

- Set up CloudWatch alarms for error rates
- Configure budget alerts for cost control
- Regularly update dependencies
- Implement feature flags for controlled rollouts

### Node.js API on EC2 with Auto Scaling

This example demonstrates how to deploy a production-grade Node.js API using EC2 instances with Auto Scaling.

#### Architecture Components:
- Amazon EC2 instances in an Auto Scaling group
- Application Load Balancer for traffic distribution
- Amazon RDS for database
- ElastiCache for Redis (session management and caching)
- Amazon CloudWatch for monitoring
- AWS Systems Manager for configuration management

#### Deployment Steps:

1. **Create a Custom AMI**:

```bash
# Launch a base EC2 instance
aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t3.micro --key-name MyKeyPair

# Connect to the instance and install dependencies
ssh -i "MyKeyPair.pem" ec2-user@<instance-ip>

# Install Node.js and other dependencies
sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
sudo yum install -y git

# Clone your application
git clone https://github.com/yourusername/your-nodejs-app.git
cd your-nodejs-app
npm install

# Set up process manager
sudo npm install -g pm2

# Create a startup script
cat > /home/ec2-user/startup.sh << 'EOL'
#!/bin/bash
cd /home/ec2-user/your-nodejs-app
pm2 start app.js
EOL

chmod +x /home/ec2-user/startup.sh

# Create an AMI from the instance
# (This would typically be done through the AWS Console or AWS CLI)
```

2. **Set Up the Infrastructure**:

```bash
# Create a launch template using the custom AMI
aws ec2 create-launch-template \
    --launch-template-name "NodeJSAppLaunchTemplate" \
    --version-description "Initial version" \
    --launch-template-data '{"ImageId":"ami-XXXXXXXXXXXXXXXXX","InstanceType":"t3.micro","KeyName":"MyKeyPair","UserData":"IyEvYmluL2Jhc2gKL2hvbWUvZWMyLXVzZXIvc3RhcnR1cC5zaA=="}'

# Create an Auto Scaling group
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name "NodeJSAppASG" \
    --launch-template "LaunchTemplateName=NodeJSAppLaunchTemplate,Version=1" \
    --min-size 2 \
    --max-size 10 \
    --desired-capacity 2 \
    --vpc-zone-identifier "subnet-XXXXXXXXXXXXXXXXX,subnet-YYYYYYYYYYYYYYYYY" \
    --target-group-arns "arn:aws:elasticloadbalancing:region:account-id:targetgroup/my-targets/XXXXXXXXXX"
```

3. **Implement Auto Scaling Policies**:

```bash
# Create a target tracking scaling policy
aws autoscaling put-scaling-policy \
    --auto-scaling-group-name "NodeJSAppASG" \
    --policy-name "CPUUtilizationScaling" \
    --policy-type "TargetTrackingScaling" \
    --target-tracking-configuration '{"PredefinedMetricSpecification":{"PredefinedMetricType":"ASGAverageCPUUtilization"},"TargetValue":70.0}'
```

4. **Set Up Monitoring and Logging**:

- Configure CloudWatch agent for system and application logs
- Set up custom metrics for application-specific monitoring
- Create dashboards for key performance indicators
- Configure alarms for critical thresholds

5. **Implement Deployment Automation**:

```yaml
# Example CodeDeploy appspec.yml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/your-nodejs-app
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_application.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_application.sh
      timeout: 300
      runas: root
```

### Flask Application on ECS Fargate

This example demonstrates how to deploy a production-grade Python Flask application using Amazon ECS with Fargate.

#### Architecture Components:
- Amazon ECS with Fargate launch type
- Application Load Balancer
- Amazon RDS for PostgreSQL
- Amazon ElastiCache for Redis
- AWS Secrets Manager for credentials
- Amazon ECR for container registry

#### Deployment Steps:

1. **Containerize the Flask Application**:

```dockerfile
# Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV FLASK_APP=app.py
ENV FLASK_ENV=production

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

2. **Build and Push the Docker Image**:

```bash
# Build the Docker image
docker build -t flask-app .

# Create an ECR repository
aws ecr create-repository --repository-name flask-app

# Authenticate Docker to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com

# Tag and push the image
docker tag flask-app:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest
```

3. **Create ECS Task Definition**:

```json
{
  "family": "flask-app",
  "networkMode": "awsvpc",
  "executionRoleArn": "arn:aws:iam::<account-id>:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::<account-id>:role/ecsTaskRole",
  "containerDefinitions": [
    {
      "name": "flask-app",
      "image": "<account-id>.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest",
      "essential": true,
      "portMappings": [
        {
          "containerPort": 5000,
          "hostPort": 5000,
          "protocol": "tcp"
        }
      ],
      "environment": [
        {
          "name": "FLASK_ENV",
          "value": "production"
        }
      ],
      "secrets": [
        {
          "name": "DATABASE_URL",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:<account-id>:secret:prod/flask-app/database-url-XXXXXX"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/flask-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ],
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512"
}
```

4. **Create ECS Service**:

```bash
# Create an ECS cluster
aws ecs create-cluster --cluster-name production-cluster

# Create a service
aws ecs create-service \
    --cluster production-cluster \
    --service-name flask-app-service \
    --task-definition flask-app:1 \
    --desired-count 2 \
    --launch-type FARGATE \
    --platform-version LATEST \
    --network-configuration "awsvpcConfiguration={subnets=[subnet-XXXXXXXXXXXXXXXXX,subnet-YYYYYYYYYYYYYYYYY],securityGroups=[sg-ZZZZZZZZZZZZZZZZZ],assignPublicIp=ENABLED}" \
    --load-balancers "targetGroupArn=arn:aws:elasticloadbalancing:us-east-1:<account-id>:targetgroup/flask-app-tg/XXXXXXXXXX,containerName=flask-app,containerPort=5000"
```

5. **Set Up CI/CD Pipeline**:

```yaml
# Example GitHub Actions workflow for ECS deployment
name: Deploy to Amazon ECS

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build, tag, and push image to Amazon ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        ECR_REPOSITORY: flask-app
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        echo "::set-output name=image::$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"

    - name: Fill in the new image ID in the Amazon ECS task definition
      id: task-def
      uses: aws-actions/amazon-ecs-render-task-definition@v1
      with:
        task-definition: task-definition.json
        container-name: flask-app
        image: ${{ steps.build-image.outputs.image }}

    - name: Deploy Amazon ECS task definition
      uses: aws-actions/amazon-ecs-deploy-task-definition@v1
      with:
        task-definition: ${{ steps.task-def.outputs.task-definition }}
        service: flask-app-service
        cluster: production-cluster
        wait-for-service-stability: true
```

### E-commerce Platform with Microservices

This example demonstrates how to build a production-grade e-commerce platform using a microservices architecture on AWS.

#### Architecture Components:
- API Gateway for frontend communication
- Lambda functions for serverless microservices
- ECS/EKS for container-based microservices
- DynamoDB for product catalog and cart
- RDS for order management
- ElastiCache for session management
- Amazon Personalize for product recommendations
- Amazon Pinpoint for marketing campaigns

#### Key Microservices:

1. **Product Catalog Service**:
   - DynamoDB for product data
   - ElastiCache for caching
   - Lambda functions for API

2. **Cart Service**:
   - DynamoDB for cart data
   - WebSocket API for real-time updates

3. **Order Service**:
   - RDS for order data
   - Step Functions for order workflow
   - SQS for order processing queue

4. **Payment Service**:
   - Integration with payment gateways
   - Lambda functions for processing
   - KMS for encryption

5. **User Service**:
   - Cognito for authentication
   - DynamoDB for user profiles

6. **Recommendation Service**:
   - Amazon Personalize for recommendations
   - Lambda functions for API

#### Implementation Highlights:

1. **API Layer**:
   - API Gateway with custom domain
   - Request validation and throttling
   - JWT authentication

2. **Data Layer**:
   - Multi-region DynamoDB for global availability
   - RDS with read replicas for scaling
   - DAX for DynamoDB acceleration

3. **Caching Strategy**:
   - Browser caching with appropriate headers
   - CDN caching for static assets
   - API response caching
   - Database query caching

4. **Security Measures**:
   - WAF for protection against common attacks
   - Shield for DDoS protection
   - GuardDuty for threat detection
   - Security Hub for compliance monitoring

5. **Observability**:
   - Centralized logging with CloudWatch Logs
   - Distributed tracing with X-Ray
   - Custom metrics for business KPIs
   - Real-time dashboards for monitoring

## Next Steps

Now that you've learned how to build production-grade applications on AWS, you're ready to explore CI/CD and DevOps practices. Continue to the [CI/CD and DevOps on AWS](../06-cicd-devops/README.md) section to learn how to automate your development and deployment workflows.

## Additional Resources

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Solutions Library](https://aws.amazon.com/solutions/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Serverless Land](https://serverlessland.com/)
- [AWS Containers](https://aws.amazon.com/containers/)