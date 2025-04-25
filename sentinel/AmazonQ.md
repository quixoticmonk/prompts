# AWS Foundational Security Best Practices Controls

| Control ID | Control Name |
|-----------|-------------|
| Account.1 | Security contact information should be provided for an AWS account |
| ACM.1 | Imported and ACM-issued certificates should be renewed after a specified time period |
| APIGateway.1 | API Gateway REST and WebSocket API execution logging should be enabled |
| APIGateway.2 | API Gateway REST API stages should be configured to use SSL certificates for backend authentication |
| APIGateway.3 | API Gateway REST API stages should have AWS X-Ray tracing enabled |
| APIGateway.4 | API Gateway should be associated with a WAF Web ACL |
| APIGateway.5 | API Gateway REST API cache data should be encrypted at rest |
| AppSync.1 | AppSync GraphQL APIs should log requests |
| AppSync.2 | AppSync GraphQL API caching should be encrypted at rest |
| AppSync.3 | AppSync GraphQL APIs should have request-level logging enabled |
| AppSync.4 | AppSync GraphQL APIs should be associated with a WAF Web ACL |
| AppSync.5 | AppSync GraphQL APIs should not have field-level resolvers that use introspection types |
| Athena.1 | Athena workgroups should be encrypted at rest |
| AutoScaling.1 | Auto Scaling groups associated with a load balancer should use load balancer health checks |
| AutoScaling.2 | Amazon EC2 Auto Scaling group should cover multiple Availability Zones |
| AutoScaling.3 | Auto Scaling group launch configurations should configure EC2 instances to require Instance Metadata Service Version 2 (IMDSv2) |
| AutoScaling.4 | Auto Scaling group launch configuration should not have a metadata response hop limit greater than 1 |
| AutoScaling.5 | Amazon EC2 instances launched using Auto Scaling group launch configurations should not have Public IP addresses |
| AutoScaling.6 | Auto Scaling groups should use multiple instance types in multiple Availability Zones |
| AutoScaling.9 | EC2 Auto Scaling groups should use EC2 launch templates |
| Backup.1 | AWS Backup recovery points should be encrypted at rest |
| CloudFormation.1 | CloudFormation stacks should be integrated with Simple Notification Service (SNS) |
| CloudFront.1 | CloudFront distributions should have a default root object configured |
| CloudFront.2 | CloudFront distributions should have origin access identity enabled |
| CloudFront.3 | CloudFront distributions should require encryption in transit |
| CloudFront.4 | CloudFront distributions should have origin failover configured |
| CloudFront.5 | CloudFront distributions should have logging enabled |
| CloudFront.6 | CloudFront distributions should have WAF enabled |
| CloudFront.7 | CloudFront distributions should use custom SSL/TLS certificates |
| CloudFront.8 | CloudFront distributions should use SNI to serve HTTPS requests |
| CloudFront.9 | CloudFront distributions should encrypt traffic to custom origins |
| CloudFront.10 | CloudFront distributions should not use deprecated SSL protocols between edge locations and custom origins |
| CloudFront.12 | CloudFront distributions should not point to non-existent S3 origins |
| CloudTrail.1 | CloudTrail should be enabled and configured with at least one multi-Region trail that includes read and write management events |
| CloudTrail.2 | CloudTrail should have encryption at rest enabled |
| CloudTrail.3 | CloudTrail log file validation should be enabled |
| CloudTrail.4 | CloudTrail trail should be integrated with CloudWatch Logs |
| CloudTrail.5 | CloudTrail trails should be configured to log data events for S3 buckets |
| CloudTrail.6 | CloudTrail trails should be configured to log data events for AWS Lambda functions |
| CloudTrail.7 | CloudTrail trails should be configured to log data events for Amazon DynamoDB tables |
| CloudWatch.1 | A log metric filter and alarm should exist for usage of the "root" user |
| CloudWatch.2 | Ensure a log metric filter and alarm exist for unauthorized API calls |
| CloudWatch.3 | Ensure a log metric filter and alarm exist for Management Console sign-in without MFA |
| CloudWatch.4 | Ensure a log metric filter and alarm exist for IAM policy changes |
| CloudWatch.5 | Ensure a log metric filter and alarm exist for CloudTrail configuration changes |
| CloudWatch.6 | Ensure a log metric filter and alarm exist for AWS Management Console authentication failures |
| CloudWatch.7 | Ensure a log metric filter and alarm exist for disabling or scheduled deletion of customer created CMKs |
| CloudWatch.8 | Ensure a log metric filter and alarm exist for S3 bucket policy changes |
| CloudWatch.9 | Ensure a log metric filter and alarm exist for AWS Config configuration changes |
| CloudWatch.10 | Ensure a log metric filter and alarm exist for security group changes |
| CloudWatch.11 | Ensure a log metric filter and alarm exist for changes to Network Access Control Lists (NACL) |
| CloudWatch.12 | Ensure a log metric filter and alarm exist for changes to network gateways |
| CloudWatch.13 | Ensure a log metric filter and alarm exist for route table changes |
| CloudWatch.14 | Ensure a log metric filter and alarm exist for VPC changes |
| CodeBuild.1 | CodeBuild Bitbucket source repository URLs should use OAuth |
| CodeBuild.2 | CodeBuild project environment variables should not contain clear text credentials |
| CodeBuild.3 | CodeBuild S3 logs should be encrypted |
| CodeBuild.4 | CodeBuild project environments should have a logging configuration |
| CodeBuild.5 | CodeBuild project environments should not have privileged mode enabled |
| Config.1 | AWS Config should be enabled |
| DMS.1 | Database Migration Service replication instances should not be public |
| DMS.6 | DMS replication instances should have automatic minor version upgrades enabled |
| DMS.7 | DMS replication tasks should have CloudWatch Logs enabled |
| DMS.8 | DMS endpoints should use SSL |
| DMS.9 | DMS replication instances should have deletion protection enabled |
| DocumentDB.1 | Amazon DocumentDB clusters should be encrypted at rest |
| DocumentDB.2 | DocumentDB clusters should have deletion protection enabled |
| DocumentDB.3 | DocumentDB cluster snapshots should not be public |
| DocumentDB.4 | DocumentDB clusters should publish audit logs to CloudWatch Logs |
| DocumentDB.5 | DocumentDB clusters should have automatic minor version upgrades enabled |
| DynamoDB.1 | DynamoDB tables should automatically scale capacity with demand |
| DynamoDB.2 | DynamoDB tables should have point-in-time recovery enabled |
| DynamoDB.3 | DynamoDB Accelerator (DAX) clusters should be encrypted at rest |
| DynamoDB.4 | DynamoDB tables should be in a backup plan |
| DynamoDB.5 | DynamoDB tables should have automatic scaling enabled or on-demand capacity |
| DynamoDB.6 | DynamoDB tables should have deletion protection enabled |
| EC2.1 | Amazon EBS snapshots should not be publicly restorable |
| EC2.2 | VPC default security group should prohibit inbound and outbound traffic |
| EC2.3 | Attached EBS volumes should be encrypted at rest |
| EC2.4 | Stopped EC2 instances should be removed after a specified time period |
| EC2.6 | VPC flow logging should be enabled in all VPCs |
| EC2.7 | EBS default encryption should be enabled |
| EC2.8 | EC2 instances should use IMDSv2 |
| EC2.9 | EC2 instances should not have a public IPv4 address |
| EC2.10 | Amazon EC2 should be configured to use VPC endpoints that are created for the Amazon EC2 service |
| EC2.15 | EC2 subnets should not automatically assign public IP addresses |
| EC2.16 | Unused network access control lists should be removed |
| EC2.17 | EC2 instances should not use multiple ENIs |
| EC2.18 | Security groups should only allow unrestricted incoming traffic for authorized ports |
| EC2.19 | Security groups should not allow unrestricted access to ports with high risk |
| EC2.20 | Both VPN tunnels for an AWS Site-to-Site VPN connection should be up |
| EC2.21 | Network ACLs should not allow ingress from 0.0.0.0/0 to port 22 or port 3389 |
| EC2.22 | Unused EC2 security groups should be removed |
| EC2.23 | Amazon EC2 Transit Gateways should not automatically accept VPC attachment requests |
| EC2.24 | Amazon EC2 paravirtual instance types should not be used |
| EC2.25 | Amazon EC2 launch templates should not assign public IPs to network interfaces |
| ECR.1 | ECR private repositories should have image scanning configured |
| ECR.2 | ECR private repositories should have tag immutability configured |
| ECR.3 | ECR repositories should have at least one lifecycle policy configured |
| ECS.1 | Amazon ECS task definitions should have secure networking modes and user definitions |
| ECS.2 | ECS services should not have public IP addresses assigned to them automatically |
| ECS.3 | ECS task definitions should not share the host's process namespace |
| ECS.4 | ECS containers should run as non-privileged |
| ECS.5 | ECS containers should be limited to read-only access to root filesystems |
| ECS.8 | Secrets should not be passed as container environment variables |
| ECS.9 | ECS task definitions should have a logging configuration |
| ECS.10 | ECS Fargate services should run on the latest Fargate platform version |
| ECS.12 | ECS clusters should use Container Insights |
| EFS.1 | Elastic File System should be configured to encrypt file data at rest using AWS KMS |
| EFS.2 | Amazon EFS volumes should be in backup plans |
| EFS.3 | EFS access points should enforce a root directory |
| EFS.4 | EFS access points should enforce a user identity |
| EKS.1 | EKS cluster endpoints should not be publicly accessible |
| EKS.2 | EKS clusters should run on a supported Kubernetes version |
| EKS.3 | EKS clusters should use encrypted Kubernetes secrets |
| EKS.8 | EKS clusters should have audit logging enabled |
| ElastiCache.1 | ElastiCache (Redis) clusters should have automatic backups enabled |
| ElastiCache.2 | ElastiCache (Redis) clusters should have automatic minor version upgrades enabled |
| ElastiCache.3 | ElastiCache replication groups should have automatic failover enabled |
| ElastiCache.4 | ElastiCache replication groups should be encrypted at rest |
| ElastiCache.5 | ElastiCache replication groups should be encrypted during transit |
| ElastiCache.6 | ElastiCache (Redis OSS) replication groups running earlier versions should be upgraded to Redis OSS version 6.0 or greater |
| ElastiCache.7 | ElastiCache clusters should not use the default subnet group |
| ElasticBeanstalk.1 | Elastic Beanstalk environments should have enhanced health reporting enabled |
| ElasticBeanstalk.2 | Elastic Beanstalk managed platform updates should be enabled |
| ElasticBeanstalk.3 | Elastic Beanstalk should be configured to send logs to CloudWatch Logs |
| ELB.1 | Application Load Balancer should be configured to redirect all HTTP requests to HTTPS |
| ELB.2 | Classic Load Balancers with SSL/HTTPS listeners should use a certificate provided by AWS Certificate Manager |
| ELB.3 | Classic Load Balancer listeners should be configured with HTTPS or TLS termination |
| ELB.4 | Application Load Balancer should be configured to drop HTTP headers |
| ELB.5 | Application and Classic Load Balancers logging should be enabled |
| ELB.6 | Application, Network, and Gateway Load Balancer deletion protection should be enabled |
| ELB.7 | Classic Load Balancers should have connection draining enabled |
| ELB.8 | Classic Load Balancers should have cross-zone load balancing enabled |
| ELB.9 | Classic Load Balancers should have connection draining enabled |
| ELB.10 | Classic Load Balancers should span multiple Availability Zones |
| ELB.12 | Application Load Balancer should be configured with defensive or strictest desync mitigation mode |
| ELB.13 | Application, Network, and Gateway Load Balancers should span multiple Availability Zones |
| ELB.14 | Classic Load Balancer should be configured with defensive or strictest desync mitigation mode |
| EMR.1 | Amazon EMR cluster primary nodes should not have public IP addresses |
| ES.1 | Elasticsearch domains should have encryption at rest enabled |
| ES.2 | Elasticsearch domains should not be publicly accessible |
| ES.3 | Elasticsearch domains should encrypt data sent between nodes |
| ES.4 | Elasticsearch domain error logging to CloudWatch Logs should be enabled |
| ES.5 | Elasticsearch domains should have audit logging enabled |
| ES.6 | Elasticsearch domains should have at least three data nodes |
| ES.7 | Elasticsearch domains should be configured with at least three dedicated primary nodes |
| ES.8 | Connections to Elasticsearch domains should be encrypted using TLS 1.2 |
| EventBridge.1 | EventBridge custom event buses should have resource-based policies attached |
| EventBridge.3 | EventBridge custom event buses should have event encryption enabled |
| FSx.1 | FSx for Lustre file systems should be configured to encrypt data at rest using AWS KMS |
| FSx.2 | FSx for Windows File Server file systems should be configured to encrypt data at rest using AWS KMS |
| GuardDuty.1 | GuardDuty should be enabled |
| IAM.1 | IAM policies should not allow full "*" administrative privileges |
| IAM.2 | IAM users should not have IAM policies attached |
| IAM.3 | IAM users' access keys should be rotated every 90 days or less |
| IAM.4 | IAM root user access key should not exist |
| IAM.5 | MFA should be enabled for all IAM users that have a console password |
| IAM.6 | Hardware MFA should be enabled for the root user |
| IAM.7 | Password policies for IAM users should have strong configurations |
| IAM.8 | Unused IAM user credentials should be removed |
| IAM.9 | MFA should be enabled for the root user |
| IAM.10 | Password policies for IAM users should have strong configurations |
| IAM.11 | Ensure IAM password policy requires at least one uppercase letter |
| IAM.12 | Ensure IAM password policy requires at least one lowercase letter |
| IAM.13 | Ensure IAM password policy requires at least one symbol |
| IAM.14 | Ensure IAM password policy requires at least one number |
| IAM.15 | Ensure IAM password policy requires minimum password length of 14 or greater |
| IAM.16 | Ensure IAM password policy prevents password reuse |
| IAM.17 | Ensure IAM password policy expires passwords within 90 days or less |
| IAM.18 | Ensure a support role has been created to manage incidents with AWS Support |
| IAM.19 | MFA should be enabled for all IAM users |
| IAM.20 | Avoid the use of the root user |
| IAM.21 | IAM customer managed policies that you create should not allow wildcard actions for services |
| IAM.22 | IAM user credentials unused for 45 days should be removed |
| IAM.23 | IAM Access Analyzer analyzer should be enabled |
| IAM.24 | IAM roles managed by services should be removed when the related service is no longer in use |
| IAM.25 | IAM SSO should be enabled for IAM user management |
| Kinesis.1 | Kinesis streams should be encrypted at rest |
| KMS.1 | IAM customer managed policies should not allow decryption actions on all KMS keys |
| KMS.2 | IAM principals should not have IAM inline policies that allow decryption actions on all KMS keys |
| KMS.3 | AWS KMS keys should not be unintentionally deleted |
| KMS.4 | AWS KMS key rotation should be enabled |
| Lambda.1 | Lambda function policies should prohibit public access |
| Lambda.2 | Lambda functions should use supported runtimes |
| Lambda.3 | Lambda functions should be in a VPC |
| Lambda.5 | VPC Lambda functions should operate in multiple Availability Zones |
| MQ.1 | Amazon MQ brokers should be configured for high availability |
| MSK.1 | MSK clusters should be encrypted during transit between broker nodes |
| Neptune.1 | Neptune DB clusters should be encrypted at rest |
| Neptune.2 | Neptune DB clusters should publish audit logs to CloudWatch Logs |
| Neptune.3 | Neptune DB cluster snapshots should not be public |
| Neptune.4 | Neptune DB clusters should have deletion protection enabled |
| Neptune.5 | Neptune DB clusters should have automatic minor version upgrades enabled |
| Neptune.6 | Neptune DB cluster snapshots should be encrypted at rest |
| Neptune.7 | Neptune DB clusters should have IAM database authentication enabled |
| Neptune.8 | Neptune DB clusters should be configured to copy tags to snapshots |
| NetworkFirewall.1 | Network Firewall firewalls should be deployed across multiple Availability Zones |
| NetworkFirewall.2 | Network Firewall logging should be enabled |
| NetworkFirewall.3 | Network Firewall policies should have at least one rule group associated |
| NetworkFirewall.4 | The default stateless action for Network Firewall policies should be drop or forward for full packets |
| NetworkFirewall.5 | The default stateless action for Network Firewall policies should be drop or forward for fragmented packets |
| NetworkFirewall.6 | Stateless Network Firewall rule group should not be empty |
| NetworkFirewall.9 | Network Firewall policies should have associated firewalls |
| Opensearch.1 | OpenSearch domains should have encryption at rest enabled |
| Opensearch.2 | OpenSearch domains should not be publicly accessible |
| Opensearch.3 | OpenSearch domains should encrypt data sent between nodes |
| Opensearch.4 | OpenSearch domain error logging to CloudWatch Logs should be enabled |
| Opensearch.5 | OpenSearch domains should have audit logging enabled |
| Opensearch.6 | OpenSearch domains should have at least three data nodes |
| Opensearch.7 | OpenSearch domains should be configured with at least three dedicated primary nodes |
| Opensearch.8 | Connections to OpenSearch domains should be encrypted using TLS 1.2 |
| Opensearch.9 | OpenSearch domains should be tagged |
| Opensearch.10 | OpenSearch domains should have the latest software updates |
| PCA.1 | AWS Private CA root certificate authority should be disabled |
| RDS.1 | RDS snapshot should be private |
| RDS.2 | RDS DB instances should prohibit public access, as determined by the PubliclyAccessible configuration |
| RDS.3 | RDS DB instances should have encryption at rest enabled |
| RDS.4 | RDS cluster snapshots and database snapshots should be encrypted at rest |
| RDS.5 | RDS DB instances should be configured with multiple Availability Zones |
| RDS.6 | Enhanced monitoring should be configured for RDS DB instances and clusters |
| RDS.7 | RDS clusters should have deletion protection enabled |
| RDS.8 | RDS DB instances should have deletion protection enabled |
| RDS.9 | RDS DB instances should publish logs to CloudWatch Logs |
| RDS.10 | IAM authentication should be configured for RDS instances |
| RDS.11 | RDS instances should have automatic backups enabled |
| RDS.12 | IAM authentication should be configured for RDS clusters |
| RDS.13 | RDS automatic minor version upgrades should be enabled |
| RDS.14 | Amazon Aurora clusters should have backtracking enabled |
| RDS.15 | RDS DB clusters should be configured for multiple Availability Zones |
| RDS.16 | RDS DB clusters should be configured to copy tags to snapshots |
| RDS.17 | RDS DB instances should be configured to copy tags to snapshots |
| RDS.18 | RDS instances should be deployed in a VPC |
| RDS.19 | Existing RDS event notifications subscriptions should be configured for critical cluster events |
| RDS.20 | Existing RDS event notifications subscriptions should be configured for critical database instance events |
| RDS.21 | An RDS event notifications subscription should be configured for critical database parameter group events |
| RDS.22 | An RDS event notifications subscription should be configured for critical database security group events |
| RDS.23 | RDS instances should not use a database engine default port |
| RDS.24 | RDS database clusters should use a custom administrator username |
| RDS.25 | RDS database instances should use a custom administrator username |
| RDS.26 | RDS DB instances should be protected by a backup plan |
| RDS.27 | RDS DB cluster snapshots should be encrypted at rest |
| RDS.28 | RDS DB clusters should be encrypted at rest |
| RDS.29 | RDS DB clusters should have deletion protection enabled |
| RDS.30 | RDS DB instances should be deployed with multiple Availability Zones configured |
| RDS.31 | RDS DB clusters should be configured to copy tags to snapshots |
| RDS.32 | RDS DB instances should be configured to copy tags to snapshots |
| RDS.33 | RDS automatic minor version upgrades should be enabled |
| RDS.34 | Aurora MySQL DB clusters should publish audit logs to CloudWatch Logs |
| RDS.35 | RDS DB clusters should have automatic minor version upgrades enabled |
| Redshift.1 | Amazon Redshift clusters should prohibit public access |
| Redshift.2 | Connections to Amazon Redshift clusters should be encrypted in transit |
| Redshift.3 | Amazon Redshift clusters should have automatic snapshots enabled |
| Redshift.4 | Amazon Redshift clusters should have audit logging enabled |
| Redshift.6 | Amazon Redshift should have automatic upgrades to major versions enabled |
| Redshift.7 | Redshift clusters should use enhanced VPC routing |
| Redshift.8 | Amazon Redshift clusters should not use the default Admin username |
| Redshift.9 | Redshift clusters should not use the default database name |
| Redshift.10 | Redshift clusters should be encrypted at rest |
| Route53.1 | Route 53 health checks should be tagged |
| Route53.2 | Route 53 public hosted zones should log DNS queries |
| S3.1 | S3 Block Public Access setting should be enabled |
| S3.2 | S3 buckets should prohibit public read access |
| S3.3 | S3 buckets should prohibit public write access |
| S3.4 | S3 buckets should have server-side encryption enabled |
| S3.5 | S3 buckets should require requests to use Secure Socket Layer |
| S3.6 | S3 Block Public Access setting should be enabled at the bucket level |
| S3.7 | S3 buckets should have cross-region replication enabled |
| S3.8 | S3 buckets should block public access |
| S3.9 | S3 bucket server access logging should be enabled |
| S3.10 | S3 buckets with versioning enabled should have lifecycle configurations |
| S3.11 | S3 buckets should have event notifications enabled |
| S3.12 | S3 access control lists (ACLs) should not be used to manage user access to buckets |
| S3.13 | S3 buckets should have lifecycle configurations |
| S3.14 | S3 buckets should have versioning enabled |
| S3.15 | S3 buckets should be configured to use Object Lock |
| S3.17 | S3 buckets should be encrypted with AWS KMS |
| S3.19 | S3 access points should have block public access settings enabled |
| S3.20 | S3 buckets should have MFA delete enabled |
| SageMaker.1 | Amazon SageMaker notebook instances should not have direct internet access |
| SageMaker.2 | SageMaker notebook instances should be launched in a custom VPC |
| SageMaker.3 | Users should not have root access to SageMaker notebook instances |
| SecretsManager.1 | Secrets Manager secrets should have automatic rotation enabled |
| SecretsManager.2 | Secrets Manager secrets configured with automatic rotation should rotate successfully |
| SecretsManager.3 | Remove unused Secrets Manager secrets |
| SecretsManager.4 | Secrets Manager secrets should be rotated within a specified number of days |
| ServiceCatalog.1 | Service Catalog portfolios should be shared with an organization |
| SES.1 | SES sending authorization policies should be restrictive |
| SNS.1 | SNS topics should be encrypted at rest using AWS KMS |
| SNS.2 | Logging of delivery status should be enabled for notification messages sent to a topic |
| SQS.1 | Amazon SQS queues should be encrypted at rest |
| SSM.1 | EC2 instances should be managed by AWS Systems Manager |
| SSM.2 | EC2 instances managed by Systems Manager should have a patch compliance status of COMPLIANT after a patch installation |
| SSM.3 | EC2 instances managed by Systems Manager should have an association compliance status of COMPLIANT |
| SSM.4 | SSM documents should not be public |
| StepFunctions.1 | Step Functions state machines should have logging turned on |
| Transfer.1 | AWS Transfer Family servers should have logging enabled |
| WAF.1 | AWS WAF Classic Global Web ACL logging should be enabled |
| WAF.2 | AWS WAF Classic Regional rule should have at least one condition |
| WAF.3 | AWS WAF Classic Regional rule groups should have at least one rule |
| WAF.4 | AWS WAF Classic Regional web ACLs should have at least one rule or rule group |
| WAF.6 | AWS WAF Classic Global rule should have at least one condition |
| WAF.7 | AWS WAF Classic Global rule groups should have at least one rule |
| WAF.8 | AWS WAF Classic Global web ACLs should have at least one rule or rule group |
| WAF.10 | AWS WAF web ACLs should have at least one rule or rule group |
| WAF.12 | AWS WAF Regional rule should have at least one condition |
| WAF.13 | AWS WAF Regional rule groups should have at least one rule |
| WAF.14 | AWS WAF Regional web ACLs should have at least one rule or rule group |
| WorkSpaces.1 | WorkSpaces should use encryption at rest for the volume |
