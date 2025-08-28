# Terraform IAM Policy Generator Prompt

## Core Instruction
Generate least-privilege Terraform execution role policies for provisioning AWS resources. Analyze the target resource and identify all dependent resources that typically need to be provisioned alongside it.

## Input Format
```
TARGET_RESOURCE: [Terraform resource type]
RESOURCE_CONFIG: [Brief description of the resource configuration]
```

## Analysis Methodology

### 1. Primary Resource Analysis
- Identify the main Terraform resource type (AWS provider vs AWSCC provider)
- Determine required AWS API permissions for CRUD operations
- Map Terraform resource attributes to AWS API actions

### 2. Dependency Chain Analysis
Identify resources commonly provisioned alongside the target:

#### Infrastructure Dependencies
- Networking (VPC, subnets, security groups)
- Storage (S3, EFS, EBS)
- Compute resources

#### Security Dependencies  
- IAM roles and policies
- KMS keys and encryption
- Secrets and parameters

#### Operational Dependencies
- CloudWatch logs and metrics
- DNS and load balancing
- Monitoring and alarms

### 3. Permission Mapping
For each identified resource, include:
- **Create**: Resource creation permissions
- **Read**: Get/Describe permissions for state management
- **Update**: Modify/Update permissions for resource changes
- **Delete**: Delete/Remove permissions for resource cleanup
- **List**: List permissions for resource discovery
- **PassRole**: For services that need to assume roles
- **CreateServiceLinkedRole**: For services that require it
- **STS**: AssumeRole, GetCallerIdentity for Terraform operations

## Example: AWS Glue Job

### Input:
```
TARGET_RESOURCE: aws_glue_job
RESOURCE_CONFIG: ETL job processing S3 data with VPC connectivity
```

### Analysis:
**Primary**: aws_glue_job
**Dependencies**: aws_glue_connection, aws_s3_bucket, aws_iam_role, aws_cloudwatch_log_group, VPC resources

### Output - Terraform IAM Policy Document:
```hcl
data "aws_iam_policy_document" "terraform_execution_policy" {
  statement {
    sid    = "TerraformSTS"
    effect = "Allow"
    actions = [
      "sts:AssumeRole",
      "sts:GetCallerIdentity"
    ]
    resources = ["*"]
  }

  statement {
    sid    = "GlueJobManagement"
    effect = "Allow"
    actions = [
      "glue:CreateJob",
      "glue:UpdateJob",
      "glue:DeleteJob",
      "glue:GetJob",
      "glue:GetJobs",
      "glue:TagResource",
      "glue:UntagResource"
    ]
    resources = [
      "arn:aws:glue:*:*:job/*"
    ]
  }

  statement {
    sid    = "GlueConnectionManagement"
    effect = "Allow"
    actions = [
      "glue:CreateConnection",
      "glue:UpdateConnection",
      "glue:DeleteConnection",
      "glue:GetConnection",
      "glue:GetConnections"
    ]
    resources = [
      "arn:aws:glue:*:*:connection/*"
    ]
  }

  statement {
    sid    = "S3Management"
    effect = "Allow"
    actions = [
      "s3:CreateBucket",
      "s3:DeleteBucket",
      "s3:PutBucketVersioning",
      "s3:PutBucketTagging",
      "s3:GetBucketLocation",
      "s3:GetBucketVersioning",
      "s3:ListBucket",
      "s3:PutObject",
      "s3:GetObject",
      "s3:DeleteObject"
    ]
    resources = [
      "arn:aws:s3:::*",
      "arn:aws:s3:::*/*"
    ]
  }

  statement {
    sid    = "IAMRoleManagement"
    effect = "Allow"
    actions = [
      "iam:CreateRole",
      "iam:DeleteRole",
      "iam:AttachRolePolicy",
      "iam:DetachRolePolicy",
      "iam:PutRolePolicy",
      "iam:DeleteRolePolicy",
      "iam:GetRole",
      "iam:GetRolePolicy",
      "iam:ListAttachedRolePolicies",
      "iam:ListRolePolicies",
      "iam:PassRole",
      "iam:TagRole",
      "iam:UntagRole"
    ]
    resources = [
      "arn:aws:iam::*:role/*"
    ]
  }

  statement {
    sid    = "CloudWatchManagement"
    effect = "Allow"
    actions = [
      "logs:CreateLogGroup",
      "logs:DeleteLogGroup",
      "logs:PutRetentionPolicy",
      "logs:DescribeLogGroups",
      "logs:ListTagsLogGroup",
      "logs:TagLogGroup",
      "logs:UntagLogGroup"
    ]
    resources = [
      "arn:aws:logs:*:*:log-group:*"
    ]
  }

  statement {
    sid    = "VPCManagement"
    effect = "Allow"
    actions = [
      "ec2:CreateVpc",
      "ec2:DeleteVpc",
      "ec2:CreateSubnet",
      "ec2:DeleteSubnet",
      "ec2:CreateSecurityGroup",
      "ec2:DeleteSecurityGroup",
      "ec2:AuthorizeSecurityGroupIngress",
      "ec2:AuthorizeSecurityGroupEgress",
      "ec2:RevokeSecurityGroupIngress",
      "ec2:RevokeSecurityGroupEgress",
      "ec2:DescribeVpcs",
      "ec2:DescribeSubnets",
      "ec2:DescribeSecurityGroups",
      "ec2:DescribeAvailabilityZones",
      "ec2:CreateTags",
      "ec2:DeleteTags"
    ]
    resources = [
      "arn:aws:ec2:*:*:vpc/*",
      "arn:aws:ec2:*:*:subnet/*",
      "arn:aws:ec2:*:*:security-group/*"
    ]
  }

  statement {
    sid    = "ServiceLinkedRoles"
    effect = "Allow"
    actions = [
      "iam:CreateServiceLinkedRole"
    ]
    resources = [
      "arn:aws:iam::*:role/aws-service-role/glue.amazonaws.com/*"
    ]
    condition {
      test     = "StringEquals"
      variable = "iam:AWSServiceName"
      values   = ["glue.amazonaws.com"]
    }
  }
}
```

## ARN Pattern Guidelines
When generating policies, use specific ARN patterns where possible:

### Common ARN Patterns:
- **IAM Roles**: `arn:aws:iam::*:role/*`
- **S3 Buckets**: `arn:aws:s3:::*` and `arn:aws:s3:::*/*`
- **CloudWatch Logs**: `arn:aws:logs:*:*:log-group:*`
- **Lambda Functions**: `arn:aws:lambda:*:*:function:*`
- **ECS Clusters**: `arn:aws:ecs:*:*:cluster/*`
- **ECS Services**: `arn:aws:ecs:*:*:service/*/*`
- **ECS Task Definitions**: `arn:aws:ecs:*:*:task-definition/*:*`
- **Glue Jobs**: `arn:aws:glue:*:*:job/*`
- **EC2 Resources**: `arn:aws:ec2:*:*:vpc/*`, `arn:aws:ec2:*:*:subnet/*`, `arn:aws:ec2:*:*:security-group/*`
- **Service-Linked Roles**: `arn:aws:iam::*:role/aws-service-role/SERVICE.amazonaws.com/*`

### When to Use Wildcards (*):
- Actions that don't support resource-level permissions (many List/Describe operations)
- STS operations (always use `*`)
- Some service-specific operations that require account-level permissions

## Application Instructions
Apply this methodology to any AWS resource by:
1. Identifying the primary resource and its AWS service
2. Mapping common dependency patterns for that resource type
3. Including all necessary CRUD permissions for Terraform operations
4. Adding iam:PassRole for services that need to assume roles
5. Including iam:CreateServiceLinkedRole with appropriate service conditions
6. Using appropriate resource ARN patterns or wildcards based on service requirements
7. Ensuring both creation and state management permissions are covered
