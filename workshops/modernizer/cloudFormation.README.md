# 🚀 DynamoDB Modernization Infrastructure

> **CloudFormation Template for MySQL to DynamoDB Migration Workshop**

A comprehensive CloudFormation template that sets up all required infrastructure components for the database modernization workshop, simulating an on-premises environment with MySQL database and providing the complete toolkit for migration to Amazon DynamoDB.

## 🔧 Infrastructure Components

This template (`modernizer-db.yaml`) creates the following resources:

### 💻 Development Environment
- **VS Code Server**: Browser-based development environment with pre-installed tools
- **User Authentication**: Secure password-protected access to development environment
- **Workshop Files**: Pre-loaded repository with workshop materials and utilities

### 🗄️ Database Infrastructure
- **MySQL Database**: Fully configured MySQL instance with sample e-commerce data
- **Database Credentials**: Auto-generated secure credentials for database access
- **Database Monitoring**: Performance logging and monitoring tools

### 🔌 Network Configuration
- **Security Groups**: Properly configured access rules for secure communication
- **VPC Endpoints**: Optimized connectivity for AWS services
- **CloudFront Distribution**: Fast, secure access to the development environment

### 👤 IAM Resources
- **DynamoDB Replication Role**: IAM role for DynamoDB streams and replication
- **Glue Service Role**: Permissions for ETL processes between MySQL and DynamoDB
- **Lambda Execution Roles**: Properly scoped permissions for utility functions

### 🔄 Migration Components
- **S3 Bucket**: Staging area for migration data
- **Glue Connections**: Pre-configured connection to MySQL database
- **Docker Support**: Container environment for compatibility testing

## 📋 Prerequisites

Before deploying this CloudFormation template, ensure you have:

1. **AWS Account Access**: Administrative permissions in your AWS account
2. **Public IP Address**: Your current public IP address for security group configuration
3. **Region Selection**: Choose a region with support for all required services
4. **CloudFormation Knowledge**: Basic understanding of AWS CloudFormation

## 🚀 Deployment Instructions

### Step 1: Deploy CloudFormation Template

1. Navigate to the AWS CloudFormation console
2. Click "Create stack" > "With new resources"
3. Upload the `modernizer-db.yaml` file
4. Complete the parameters form with required information:
   - Environment name
   - VS Code user credentials
   - Instance type and volume size
   - Your public IP address for access control
```
We are not doing any of this right?
### Step 2: Post-Deployment Configuration

> ⚠️ **CRITICAL**: Wait at least 5 minutes after CloudFormation completes before proceeding!

1. **Update Glue Connection**:
   - Go to AWS Glue console > Connections
   - Edit the `mysql-modernizer-connection`
   - Update the password using the value from CloudFormation outputs

2. **Configure MCP Servers**:
   - Update `tools/config.json` with outputs from CloudFormation
   - Configure DynamoDB, AWS Data Processing, and MySQL MCP servers
   - Ensure all servers use the same AWS region
```
===
```
<all this is in workshop https://catalog.workshops.aws/dynamodb-labs/en-US/modernizer/00-preparation, I need not talk right?
## 🛠️ Using the Infrastructure

### 🖥️ Accessing VS Code Server

1. Find the VS Code Server URL in CloudFormation outputs
2. Navigate to the URL in your browser
3. Log in using the credentials from Secrets Manager (available in outputs)
4. The workshop files are pre-loaded in `/home/participant/modernizer`

### 🗄️ Connecting to MySQL Database

The MySQL database is pre-configured with the sample e-commerce database:

```bash
# Connect using the MySQL client
mysql -h <PUBLIC_IP> -u dbuser -p<PASSWORD> online_shopping_store

# Connection details can be found in:
# - CloudFormation outputs
# - `/home/participant/.bashrc` environment variables
```
```
### 🔄 Running Migration Tools

The template sets up all required permissions and connections for migration:

1. AWS Glue ETL jobs can be created through the AWS console
2. S3 bucket is provisioned for ETL staging
3. DynamoDB tables can be created with appropriate IAM permissions
```

## 🔍 Infrastructure Details

### 📊 Resource Specifications

| Component | Specification | Notes |
|-----------|---------------|-------|
| EC2 Instance | t4g.large | ARM-based for cost optimization |
| Storage | 40 GB gp3 | Encrypted EBS volume |
| MySQL | 8.0+ | Community edition |
| Network | Default VPC | VPC endpoints for optimization |
| Python | 3.13 | Latest version for compatibility |
| Node.js | v18 | LTS version for stability |

### 🔒 Security Features

- **IAM Least Privilege**: Roles follow least privilege principle
- **Secret Management**: Credentials stored in AWS Secrets Manager
- **Network Security**: Restricted access to your IP address only
- **Encrypted Storage**: EBS volumes encrypted by default
- **CloudFront Security**: HTTPS connections for secure access

## 🧪 Monitoring & Troubleshooting
```
(do we need this logs and monitoring section)
### 📈 Logs & Monitoring

- **CloudWatch Agent**: Pre-installed for log and metric collection
- **MySQL Slow Query Log**: Enabled at `/var/log/mysql/mysql-slow.log`
- **MySQL General Log**: Available at `/var/log/mysql/mysql-general.log`
- **Performance Schema**: Enabled for detailed database monitoring
```
### 🔧 Troubleshooting

**VS Code Server Access Issues**:
1. Verify your IP address matches the allowed IP in security group
2. Check CloudFront distribution status in AWS console
3. Wait 5+ minutes after deployment for all services to initialize

**Database Connection Problems**:
1. Confirm MySQL service is running on the instance
2. Verify database credentials from CloudFormation outputs
3. Check security group rules allow traffic on port 3306
4. Update Glue connection with correct password

**ETL Job Failures**:
1. Verify Glue connection password is updated
2. Ensure VPC endpoints are properly configured
3. Check IAM permissions for Glue service role
4. Monitor CloudWatch logs for specific error messages

## 📚 Additional Resources

- [DynamoDB Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [AWS Glue User Guide](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)
- [MySQL 8.0 Reference Manual](https://dev.mysql.com/doc/refman/8.0/en/)
- [VS Code Server Documentation](https://github.com/coder/code-server)

```
Idk if this is true
## 🛡️ Security Considerations

This template is designed for workshop/learning environments and includes:

- Temporary credentials with automatic rotation
- Network access restricted to specified IP addresses
- IAM roles with appropriate permission boundaries
- Encrypted data storage and transmission

For production deployments, additional security measures should be implemented.
```

## 🚮 Cleanup Instructions
```
Deleting stack should delete the resources and for DDB&Glue resources we are having a seperate script
```

To remove all resources created by this template:

1. Delete any data in the S3 bucket created by the template
2. Delete the CloudFormation stack from the AWS console
3. Verify all resources have been terminated properly

All resources are configured with appropriate deletion policies to ensure complete cleanup.
