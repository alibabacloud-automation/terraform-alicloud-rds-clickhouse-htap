# Complete Example

This example demonstrates how to use the RDS ClickHouse HTAP module to create a complete HTAP (Hybrid Transactional/Analytical Processing) solution.

## Overview

This example creates:
- A VPC with a VSwitch in the specified availability zone
- A security group for network access control
- An RDS MySQL instance for transactional processing
- A ClickHouse cluster for analytical processing
- An ECS instance for application deployment
- ECS commands for automated HTAP installation

## Usage

To run this example you need to execute:

```bash
$ terraform init
$ terraform plan
$ terraform apply
```

Note that this example may create resources which will incur costs. Run `terraform destroy` when you don't need these resources.

## Important Notes

1. **Passwords**: You must provide strong passwords for the RDS, ClickHouse, and ECS instances. The passwords must be 8-30 characters long and contain at least three of the following: uppercase letters, lowercase letters, numbers, and special characters (!@#$%^&*()_+-=).

2. **Custom Script**: You can optionally provide a custom ECS command script via the `custom_ecs_script` variable. If not provided, the default HTAP installation script will be used.

3. **Region**: Make sure to choose a region that supports all the required services (RDS MySQL, ClickHouse, ECS).

## Example Usage

```bash
# Set required variables
export TF_VAR_rds_password="YourRdsPassword123!"
export TF_VAR_clickhouse_password="YourClickHousePassword123!"
export TF_VAR_ecs_password="YourEcsPassword123!"

# Initialize and apply
terraform init
terraform plan
terraform apply
```
