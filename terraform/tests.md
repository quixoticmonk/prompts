# Terraform Unit Test Guide

## Overview
Create comprehensive unit tests for Terraform modules using Terraform's built-in testing framework (v1.8.0+) to validate functionality without creating actual infrastructure. Tests verify that your module behaves as expected during the plan phase without provisioning resources.

## Key Requirements

### Test File Structure
- Name test files with `.tftest.hcl` extension
- Place tests in a `tests/` directory at the module root
- Create separate test files for different scenarios (e.g., `basic.tftest.hcl`, `advanced_config.tftest.hcl`)
- Each test file should focus on a specific aspect of functionality

### Test Execution Workflow
- **ALWAYS run `terraform test` before starting development**
- **Run `terraform test` after each new test is added**
- **Fix any execution errors immediately**
- Use `-filter` flag to run specific tests during development
- Use `-verbose` flag for detailed output when debugging

## Test example structure

```hcl
# Global variables for all tests
variables {
  # General configuration
  prefix          = "test"
  primary_region  = "us-east-1"
  organization_id = "o-abcdefghij"
  account_id      = "123456789012"

  # Lambda function configuration
  lambda_runtime              = "python3.13"
  lambda_timeout              = 60
  lambda_reserved_concurrency = 10

  # Lambda function IAM roles
  service_a_lambda_function_iam_role_arn = "arn:aws:iam::123456789012:role/test-lambda-service-a"
  service_b_lambda_function_iam_role_arn = "arn:aws:iam::123456789012:role/test-lambda-service-b"

  # Network configuration
  lambda_function_subnet_ids         = ["subnet-12345678", "subnet-87654321"]
  lambda_function_security_group_ids = ["sg-12345678"]

  # Secret ARNs
  service_a_secret_arn = "arn:aws:secretsmanager:us-east-1:123456789012:secret:test-service-a-123456"
  service_b_secret_arn = "arn:aws:secretsmanager:us-east-1:123456789012:secret:test-service-b-123456"
}

# Mock AWS provider to avoid needing real credentials
mock_provider "aws" {
  override_during = plan

  # Mock AWS Lambda KMS key
  mock_data "aws_kms_key" {
    defaults = {
      arn = "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"
    }
  }
}

# Override data sources
override_data {
  target = data.aws_region.current
  values = {
    name = "us-east-1"
  }
}

override_data {
  target = data.aws_kms_key.aws_lambda
  values = {
    arn = "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"
  }
}

# Mock the archive files for Lambda functions
override_data {
  target = data.archive_file.lambda_function_packages["service_a"]
  values = {
    output_path         = "/tmp/lambda_package_service_a.zip"
    output_base64sha256 = "mock-base64-sha256-value-for-service-a"
  }
}

override_data {
  target = data.archive_file.lambda_function_packages["service_b"]
  values = {
    output_path         = "/tmp/lambda_package_service_b.zip"
    output_base64sha256 = "mock-base64-sha256-value-for-service-b"
  }
}

# Test Lambda Functions Creation
run "test_lambda_functions_creation" {
  command = plan

  # Test assertions for Lambda functions
  assert {
    condition     = aws_lambda_function.functions["service_a"].function_name == "${var.prefix}-lambda-service-a"
    error_message = "Service A Lambda function name is incorrect"
  }

  assert {
    condition     = aws_lambda_function.functions["service_b"].function_name == "${var.prefix}-lambda-service-b"
    error_message = "Service B Lambda function name is incorrect"
  }

  # Test IAM roles for Lambda functions
  assert {
    condition     = aws_lambda_function.functions["service_a"].role == var.service_a_lambda_function_iam_role_arn
    error_message = "Service A Lambda function IAM role is incorrect"
  }

  assert {
    condition     = aws_lambda_function.functions["service_b"].role == var.service_b_lambda_function_iam_role_arn
    error_message = "Service B Lambda function IAM role is incorrect"
  }

  # Test common configuration for Lambda functions
  assert {
    condition     = aws_lambda_function.functions["service_a"].runtime == var.lambda_runtime
    error_message = "Service A Lambda function runtime is incorrect"
  }

  assert {
    condition     = aws_lambda_function.functions["service_b"].runtime == var.lambda_runtime
    error_message = "Service B Lambda function runtime is incorrect"
  }
}

# Test Lambda Environment Variables
run "test_lambda_environment_variables" {
  command = plan

  # Test assertions for Lambda environment variables
  assert {
    condition     = aws_lambda_function.functions["service_a"].environment[0].variables["SECRET_ARN"] == var.service_a_secret_arn
    error_message = "Service A Lambda function SECRET_ARN environment variable is incorrect"
  }

  assert {
    condition     = aws_lambda_function.functions["service_b"].environment[0].variables["SECRET_ARN"] == var.service_b_secret_arn
    error_message = "Service B Lambda function SECRET_ARN environment variable is incorrect"
  }
}

# Test VPC Configuration
run "test_vpc_configuration" {
  command = plan

  # Test assertions for VPC configuration
  assert {
    condition     = contains(aws_lambda_function.functions["service_a"].vpc_config[0].subnet_ids, var.lambda_function_subnet_ids[0])
    error_message = "Service A Lambda function subnet IDs are incorrect"
  }

  assert {
    condition     = contains(aws_lambda_function.functions["service_a"].vpc_config[0].security_group_ids, var.lambda_function_security_group_ids[0])
    error_message = "Service A Lambda function security group IDs are incorrect"
  }
}

# Test SQS Queue Configuration
run "test_sqs_queue_configuration" {
  command = plan

  # Test assertions for SQS queues
  assert {
    condition     = aws_sqs_queue.lambda_dlq["service_a"].name == "${var.prefix}-lambda-service-a-dlq"
    error_message = "Service A Lambda DLQ name is incorrect"
  }

  assert {
    condition     = aws_sqs_queue.lambda_dlq["service_b"].name == "${var.prefix}-lambda-service-b-dlq"
    error_message = "Service B Lambda DLQ name is incorrect"
  }

  # Test SQS queue encryption
  assert {
    condition     = aws_sqs_queue.lambda_dlq["service_a"].sqs_managed_sse_enabled == true
    error_message = "Service A SQS DLQ encryption is not enabled"
  }

  assert {
    condition     = aws_sqs_queue.lambda_dlq["service_b"].sqs_managed_sse_enabled == true
    error_message = "Service B SQS DLQ encryption is not enabled"
  }
}
```



## Mock Limitations
Remember that override blocks have strict limitations:
- No functions (like `file()`, `templatefile()`)
- No variable references (`var.xxx`)
- No path references (`path.module`)
- No local values or other expressions
- Values must be literal constants
- No labels are expected for override_data blocks.
- override_data blocks must specify a target address.

## Testing Strategy

### 1. Test Coverage Priorities
- Test all resource types created by the module
- Verify conditional resource creation logic
- Test naming conventions and prefixing
- Validate security configurations
- Test edge cases and optional features

### 2. Test Organization
- Group related tests in logical run blocks
- Progress from simple to complex configurations
- Test each major feature in isolation
- Create comprehensive tests for critical resources

### 3. Assertion Best Practices
- Test only values known during plan phase
- Use multiple assertions to verify different aspects
- Provide clear error messages
- Avoid testing computed values (IDs, ARNs, timestamps)
- Focus on configuration values, not resource creation

## Example Test Suite Structure

```
terraform-aws-glue/
├── main.tf
├── variables.tf
├── outputs.tf
├── tests/
│   ├── basic.tftest.hcl        # Basic functionality
│   ├── job_types.tftest.hcl    # Different job type configurations
│   ├── security.tftest.hcl     # Security configurations
│   └── edge_cases.tftest.hcl   # Edge cases and optional features
```

## Test Execution Commands
```bash
# Run all tests
terraform test

# Run specific test file
terraform test -filter=tests/basic.tftest.hcl

# Run with detailed output
terraform test -verbose
```

By following these guidelines, you'll create robust tests that validate your module's functionality without creating actual infrastructure, ensuring reliability and correctness across different configurations.
