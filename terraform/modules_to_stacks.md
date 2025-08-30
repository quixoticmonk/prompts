# Terraform Module to Stack Conversion Prompt

## Agent Role
You are a Terraform expert with deep knowledge of both HCP Terraform workspaces and Terraform Stacks. You understand the architectural differences between traditional workspace-based deployments and the newer stack-based approach. You can efficiently convert existing Terraform configurations from workspace patterns to stack patterns while maintaining best practices for multi-environment deployments.

## Task: 
* Convert the following Terraform configuration from traditional modules to Terraform Stacks structure:

### Requirements:
1. **Separate .tfcomponent.hcl files** for different concerns:
   - `providers.tfcomponent.hcl` - Provider configurations, requirements, and assume role setup
   - `variables.tfcomponent.hcl` - Stack-level variables  
   - `components.tfcomponent.hcl` - Component definitions
   - `deployments.tfdeploy.hcl` - Environment-specific configurations

2. **Provider Configuration**:
   - `required_providers` is only needed once in `providers.tfcomponent.hcl`
   - Include AWS role assumption configuration in providers
   - Providers are automatically available to all components

3. **Component Structure**:
   - Each module becomes a component referencing modules directly
   - **PRESERVE ORIGINAL MODULE SOURCES**: Use `source` to reference terraform registry modules (e.g., `terraform-aws-modules/vpc/aws`) or local modules (e.g., `./modules/custom-vpc`) exactly as they appear in the original configuration - do not recreate modules as local resources
   - Components reference each other using `component.<name>.<output>`
   - Components contain: source, version (for external modules), and inputs
   - Use compatible module versions that work with the current AWS provider

4. **Authentication & Multi-Environment**:
   - Configure `assume_role_with_web_identity` block in AWS provider with structure: `provider "aws" "source" { config { assume_role_with_web_identity { role_arn = var.role_arn; web_identity_token = var.identity_token } } }`
   - Define `identity_token "aws"` block in `deployments.tfdeploy.hcl` with audience `["aws.workload.identity"]`
   - Reference identity token in deployment inputs as `identity_token = identity_token.aws.jwt`
   - Different deployments for dev/prod environments with environment-specific role ARNs and variable overrides
   - identity_token variable should be set as ephemeral

### Reference Structure:
Follow the pattern from: https://github.com/quixoticmonk/s3-crossregion-stack-example


### Expected Output:
- .terraform-version (ALWAYS check and use the actual current Terraform version installed on the system)
- providers.tfcomponent.hcl (with assume_role configuration)
- variables.tfcomponent.hcl (including aws_role_arn variable)
- components.tfcomponent.hcl (components only, preserving original module sources)
- deployments.tfdeploy.hcl (with role ARNs per environment)

### Validation:
After creating the stack configuration files:
1. Run `terraform stacks init` to initialize the stack
2. Run `terraform stacks validate` to validate the configuration
3. Fix any validation errors or issues that arise
