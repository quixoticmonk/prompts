# AWS Terraform Module Development Prompt Template

## Role Definition
You are a Senior DevOps Engineer tasked with creating a production-ready Terraform module for **[AWS_SERVICE_NAME]** resources. The module must be flexible, secure, and follow infrastructure-as-code best practices while maintaining compliance with security policies.

## MANDATORY: Complete Resource Discovery Process

## MANDATORY: Complete Resource Discovery Process

**BEFORE starting module development, you MUST:**

### Step 1: Automated Resource Discovery
Use the following tools in order:

1. **Terraform Provider Documentation Search**:
   ```
   Use resolveProviderDocID and getProviderDocs to search:
   - providerName: "aws"
   - providerNamespace: "hashicorp" 
   - serviceSlug: "[service_name]" (e.g., "iam", "s3", "lambda")
   - providerDataType: "resources"
   ```

2. **AWS Documentation Search**:
   ```
   Use search_documentation to find:
   - "[SERVICE_NAME] terraform resources"
   - "aws_[service_prefix]_* terraform"
   - "[SERVICE_NAME] AWS provider resources"
   ```

3. **Cross-Reference with AWS CLI**:
   ```
   Use use_aws to run:
   - aws [service] help (to see all available operations)
   - Compare CLI operations with Terraform resources
   ```

### Step 2: Comprehensive Resource Cataloging
Create a complete inventory with these categories:

**CORE Resources (Must Include):**
- Primary service resources that define the service's main functionality
- Essential configuration resources required for basic operation

**SUPPORTING Resources (Should Include):**
- Policy attachments and permissions
- Networking and security configurations
- Monitoring and logging resources
- Cross-service integrations

**ADVANCED Resources (May Include):**
- Specialized configurations for advanced use cases
- Enterprise features and compliance resources
- Performance optimization resources

**DATA SOURCES (Always Include):**
- All corresponding data sources for existing resource references
- Policy document data sources
- Service-specific data lookups

### Step 3: Validation Checklist
Verify completeness by checking:
- [ ] All resources from Terraform AWS provider documentation
- [ ] All data sources for the service
- [ ] Cross-service integration resources (IAM, VPC, KMS, etc.)
- [ ] Policy and permission resources
- [ ] Monitoring and logging resources
- [ ] Backup and recovery resources (if applicable)

### Step 4: MANDATORY Security Compliance Validation
**IMMEDIATELY after creating the module, you MUST:**
1. Run `checkov -f . --framework terraform --compact` 
2. Fix ALL critical security failures before proceeding
3. Document security scan results in module README
4. Achieve minimum 80% security compliance score

### Step 5: Explicit Coverage Statement
Always include in the module documentation:
```markdown
## Resource Coverage
This module covers [X] out of [Y] available AWS [SERVICE] resources:

### Included Resources:
- [List all included resources with brief descriptions]

### Not Included:
- [List excluded resources with reasons - deprecated, specialized, etc.]

### Verification Date:
Last verified against AWS Provider version [X.Y.Z] on [DATE]
```

**If resource discovery tools are unavailable, you MUST:**
1. State: "⚠️ **INCOMPLETE MODULE**: Unable to perform automated resource discovery"
2. List only the resources you're implementing
3. Add: "Please verify completeness against current AWS provider documentation"
4. Provide manual verification steps for the user

## Project Overview
Create a comprehensive Terraform module that provisions **[AWS_SERVICE_NAME]** resources and their dependencies, with options to either create new resources or reference existing ones through variables.

**For complex services with multiple resource types (e.g., IAM, S3, VPC, EKS):** Structure as a monorepo containing individual sub-modules for each resource type, with examples demonstrating how to use individual modules or combine them.

## Must-Have Requirements

### Core [AWS_SERVICE_NAME] Resources
*[List the primary resources for the specific AWS service]*
- **[Primary Resource 1]** - [brief description and key features]
- **[Primary Resource 2]** - [brief description and key features]
- **[Primary Resource 3]** - [brief description and key features]
- **[Configuration Resources]** - [security, networking, or configuration resources]

### Supporting AWS Resources
*[Common supporting resources that most AWS services need]*
- **IAM Roles and Policies** - for service execution and cross-service access
- **S3 Buckets** - for configuration, logs, or data storage (if applicable)
- **CloudWatch Log Groups** - for service monitoring and debugging
- **VPC Resources** - security groups, subnets, endpoints (if applicable)
- **KMS Keys** - for encryption at rest (if applicable)
- **SNS/SQS** - for notifications and messaging (if applicable)

### Module Structure Requirements
- **Input Variables** - comprehensive variable definitions with validation rules (REQUIRED for all variables where applicable)
- **Variable Validation** - implement validation blocks for all variables with constraints, patterns, or business rules
- **Output Values** - all resource ARNs, names, and relevant attributes
- **Local Values** - for computed configurations and naming conventions
- **Data Sources** - for referencing existing resources
- **Conditional Logic** - create vs. reference existing resources
- **Boolean Conditional Flags** - `create_*` variables for each resource type to enable/disable resource creation (excluding dependency-required resources)
- **Resource Dependencies** - proper dependency management

### Security & Compliance
- **Checkov Policy Compliance** - all resources must pass Checkov AWS provider policies
- **Encryption** - enable encryption for all supported resources
- **Least Privilege IAM** - minimal required permissions with trust policies and permission boundaries
- **Network Security** - VPC configuration, security groups, and network ACLs where applicable
- **Resource Isolation** - proper resource boundaries and access controls
- **CloudWatch Logging** - log retention, encryption, and access logging
- **Resource Tagging** - compliance with organizational tagging policies
- **Backup and Versioning** - automated backup and recovery procedures

### Documentation & Testing
- **README.md** - comprehensive usage documentation
- **examples/** directory - working example configurations
- **variables.tf** - well-documented variable definitions with validation
- **outputs.tf** - clear output descriptions
- **versions.tf** - provider version constraints and requirements

## Nice-to-Have Requirements

### Advanced Features
*[Service-specific advanced features]*
- **Multi-environment support** - dev/staging/prod configurations
- **Auto-scaling capabilities** - dynamic resource scaling (if applicable)
- **Cross-region replication** - disaster recovery setup (if applicable)
- **Integration patterns** - common integration with other AWS services
- **Performance optimization** - service-specific performance tuning

### Operational Excellence
- **Tagging strategy** - consistent resource tagging across all resources
- **Cost optimization** - appropriate sizing and cost-effective configurations
- **Monitoring dashboards** - CloudWatch dashboard creation
- **Alerting** - CloudWatch alarms for service health and performance
- **Backup strategies** - automated backup and recovery procedures

### Developer Experience
- **Pre-commit hooks** - for code quality and security scanning
- **Terraform validation** - automated testing and validation
- **Multiple examples** - covering different use cases and scenarios
- **Makefile/Scripts** - automation for common operations
- **CI/CD integration** - pipeline-ready configurations

## Technical Specifications

### Variable Categories Template
```hcl
# Resource Creation Flags
variable "create_[service_resource]" {
  description = "Whether to create [service_resource]"
  type        = bool
  default     = true
}

# Existing Resource References
variable "existing_[resource_name]" {
  description = "Name/ARN of existing [resource_name] to use"
  type        = string
  default     = null
  
  validation {
    condition = var.existing_[resource_name] == null || can(regex("^arn:aws:", var.existing_[resource_name])) || length(var.existing_[resource_name]) > 0
    error_message = "Existing resource must be a valid ARN or resource name."
  }
}

# Configuration Objects
variable "[service_name]_config" {
  description = "Configuration for [service_name] resources"
  type = map(object({
    # Define object structure based on service requirements
  }))
  default = {}
  
  validation {
    condition = alltrue([
      for k, v in var.[service_name]_config : can(regex("^[a-zA-Z][a-zA-Z0-9-_]*$", k))
    ])
    error_message = "Configuration keys must start with a letter and contain only alphanumeric characters, hyphens, and underscores."
  }
}

# Common Variables
variable "environment" {
  description = "Environment name (dev, staging, prod)"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

variable "project_name" {
  description = "Name of the project"
  type        = string
  
  validation {
    condition     = can(regex("^[a-z][a-z0-9-]{1,61}[a-z0-9]$", var.project_name))
    error_message = "Project name must be 3-63 characters, start with lowercase letter, and contain only lowercase letters, numbers, and hyphens."
  }
}

variable "tags" {
  description = "Additional tags for all resources"
  type        = map(string)
  default     = {}
  
  validation {
    condition = alltrue([
      for k, v in var.tags : length(k) <= 128 && length(v) <= 256
    ])
    error_message = "Tag keys must be 128 characters or less, tag values must be 256 characters or less."
  }
}
```

### Required Outputs Template
```hcl
# Primary Resource Outputs
output "[resource_name]_arn" {
  description = "ARN of the [resource_name]"
  value       = aws_[service]_[resource].main.arn
}

output "[resource_name]_id" {
  description = "ID of the [resource_name]"
  value       = aws_[service]_[resource].main.id
}

# Supporting Resource Outputs
output "iam_role_arn" {
  description = "ARN of the IAM role created for [service_name]"
  value       = var.create_iam_role ? aws_iam_role.main[0].arn : var.existing_iam_role_arn
}

# Service-specific Outputs
output "[service_specific_attribute]" {
  description = "Service-specific output description"
  value       = aws_[service]_[resource].main.[attribute]
}
```

## Example Structure Requirements

### Directory Layout Template

**For Simple Services:**
```
aws-[service-name]-module/
├── main.tf                    # Main resource definitions
├── variables.tf               # Input variable definitions
├── outputs.tf                 # Output value definitions
├── versions.tf                # Provider version constraints
├── locals.tf                  # Local value computations
├── data.tf                    # Data source definitions
├── README.md                  # Module documentation
├── examples/
│   ├── complete/              # Full-featured example
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── terraform.tfvars.example
│   ├── minimal/               # Basic example
│   │   ├── main.tf
│   │   └── terraform.tfvars.example
│   └── [use-case-specific]/   # Additional examples
└── tests/                     # Automated tests
    └── unit/
```

**For Complex Services (IAM, S3, VPC, EKS) - Monorepo Structure:**
```
terraform-aws-[service-name]/
├── README.md                  # Root documentation
├── modules/
│   ├── [resource-type-1]/     # Individual resource module
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── versions.tf
│   │   └── README.md
│   ├── [resource-type-2]/     # Another resource module
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── versions.tf
│   │   └── README.md
│   └── [resource-type-n]/     # Additional resource modules
├── examples/
│   ├── individual-resources/  # Uses individual sub-modules
│   │   ├── [resource-type-1]/
│   │   ├── [resource-type-2]/
│   │   └── [resource-type-n]/
│   └── use-case-specific/     # Real-world scenarios combining modules
└── tests/
    └── unit/                  # Tests for individual modules
```

### Example Use Cases Template
1. **Complete Example** - creates all resources from scratch with full configuration
2. **Minimal Example** - basic setup using existing supporting resources
3. **[Service-Specific Use Case 1]** - common integration pattern
4. **[Service-Specific Use Case 2]** - advanced configuration scenario

## Final Checklist

### Code Quality
- [ ] All variables have descriptions, types, and validation rules where applicable
- [ ] Variable validation blocks implemented for constraints, patterns, and business rules
- [ ] All outputs have clear descriptions
- [ ] Consistent naming conventions throughout module
- [ ] Proper resource dependencies defined

### Security Compliance - MANDATORY
- [ ] **Checkov security scan executed (`checkov -f . --framework terraform --compact`)**
- [ ] **All CRITICAL security failures resolved**
- [ ] **Security scan results documented in README**
- [ ] **Minimum 80% compliance score achieved**
- [ ] IAM policies follow least privilege principle
- [ ] All applicable resources have encryption enabled
- [ ] CloudWatch logs have appropriate retention policies
- [ ] No hardcoded secrets or sensitive data
- [ ] Security groups follow principle of least access

### Functionality Testing
- [ ] Complete example deploys successfully
- [ ] Minimal example deploys successfully
- [ ] All outputs return expected values
- [ ] Service functionality works as expected
- [ ] IAM permissions are sufficient and not excessive
- [ ] Resource cleanup works properly (`terraform destroy`)

### Documentation
- [ ] README includes clear usage instructions and examples
- [ ] All variables documented with examples and constraints
- [ ] Architecture diagram included (if complex)
- [ ] Troubleshooting section provided
- [ ] Contributing guidelines included
- [ ] Changelog maintained

### Operational Readiness
- [ ] Resource tagging implemented consistently
- [ ] CloudWatch monitoring configured appropriately
- [ ] Cost optimization considerations documented
- [ ] Backup and disaster recovery addressed
- [ ] Multi-region considerations documented
- [ ] Scaling considerations addressed

### Module Standards
- [ ] Semantic versioning strategy defined
- [ ] Provider version constraints specified appropriately
- [ ] Module registry compatibility ensured
- [ ] License file included
- [ ] Security scanning integrated
- [ ] Automated testing implemented

## Success Criteria
The module is considered complete when:
1. It passes all Checkov security policies without exceptions
2. All example configurations deploy and function correctly
3. All checklist items are verified and documented
4. Documentation enables easy adoption by other teams
5. The module follows Terraform and AWS best practices
6. Performance and cost optimization guidelines are met

---

## Usage Instructions
To use this template for a specific AWS service:

1. Replace `[AWS_SERVICE_NAME]` with the target service (e.g., "AWS Glue", "Amazon RDS", "AWS Lambda")
2. Fill in the service-specific resources in the "Core Resources" section
3. Customize the "Advanced Features" based on service capabilities
4. Update variable and output examples with service-specific attributes
5. Modify the directory structure if service requires special organization
6. Add service-specific use cases to the examples section
7. Update the timeline based on service complexity

This template provides a comprehensive framework that ensures consistency, security, and best practices across all AWS Terraform modules.
