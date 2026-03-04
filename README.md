Alibaba Cloud RDS ClickHouse HTAP Solution Terraform Module

# terraform-alicloud-rds-clickhouse-htap

English | [简体中文](https://github.com/alibabacloud-automation/terraform-alicloud-rds-clickhouse-htap/blob/main/README-CN.md)

Terraform module which creates a complete HTAP (Hybrid Transactional/Analytical Processing) solution using RDS MySQL and ClickHouse on Alibaba Cloud. This module implements the [RDS and ClickHouse enable an all-in-one HTAP solution](https://www.aliyun.com/solution/tech-solution/rdsclickhouse-htap), which involves the creation and deployment of resources such as Virtual Private Cloud (VPC), Virtual Switch (VSwitch), Elastic Compute Service (ECS), RDS Database (RDS), ClickHouse Database (ClickHouse).

## Usage

This module creates a complete HTAP infrastructure including VPC, RDS MySQL for transactional processing, ClickHouse for analytical processing, and ECS for application deployment with automated installation scripts.

```terraform
module "rds_clickhouse_htap" {
  source = "alibabacloud-automation/rds-clickhouse-htap/alicloud"

  # VPC configuration
  vpc_config = {
    cidr_block = "192.168.0.0/16"
    vpc_name   = "htap-vpc"
  }

  # VSwitch configuration - zone_id is required
  vswitch_config = {
    cidr_block   = "192.168.0.0/24"
    zone_id      = "cn-hangzhou-h"
    vswitch_name = "htap-vswitch"
  }

  # RDS account configuration
  rds_db_account_config = {
    account_name     = "rds_user"
    account_password = "YourRdsPassword123!"
  }

  # ClickHouse account configuration
  clickhouse_account_config = {
    account_name     = "ck_user"
    account_password = "YourClickHousePassword123!"
  }

  # ECS instance configuration - image_id is required
  ecs_instance_config = {
    instance_type = "ecs.e-c1m2.large"
    password      = "YourEcsPassword123!"
    image_id      = "centos_7_9_x64_20G_alibase_20240403.vhd"
  }
}
```

## Examples

* [Complete Example](https://github.com/alibabacloud-automation/terraform-alicloud-rds-clickhouse-htap/tree/main/examples/complete)

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_alicloud"></a> [alicloud](#requirement\_alicloud) | >= 1.212.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_alicloud"></a> [alicloud](#provider\_alicloud) | >= 1.212.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [alicloud_click_house_account.default](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/click_house_account) | resource |
| [alicloud_click_house_db_cluster.click_house](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/click_house_db_cluster) | resource |
| [alicloud_db_account.default](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/db_account) | resource |
| [alicloud_db_database.database](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/db_database) | resource |
| [alicloud_db_instance.rds_instance](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/db_instance) | resource |
| [alicloud_ecs_command.run_tpcc_alicloud_ecs_command](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/ecs_command) | resource |
| [alicloud_ecs_invocation.run_tpcc_alicloud_ecs_invocation](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/ecs_invocation) | resource |
| [alicloud_instance.ecs_instance](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/instance) | resource |
| [alicloud_security_group.security_group](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/security_group) | resource |
| [alicloud_vpc.vpc](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/vpc) | resource |
| [alicloud_vswitch.vswitch](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/vswitch) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_clickhouse_account_config"></a> [clickhouse\_account\_config](#input\_clickhouse\_account\_config) | The parameters of ClickHouse account. The attributes 'account\_name', 'account\_password', 'type' are required. | <pre>object({<br/>    account_name     = string<br/>    account_password = string<br/>    type             = string<br/>  })</pre> | <pre>{<br/>  "account_name": "ck_user",<br/>  "account_password": null,<br/>  "type": "Super"<br/>}</pre> | no |
| <a name="input_clickhouse_cluster_config"></a> [clickhouse\_cluster\_config](#input\_clickhouse\_cluster\_config) | The parameters of ClickHouse cluster. The attributes 'db\_cluster\_version', 'category', 'db\_cluster\_class', 'db\_cluster\_network\_type', 'db\_node\_group\_count', 'payment\_type', 'db\_node\_storage', 'storage\_type', and 'db\_cluster\_description' are required. | <pre>object({<br/>    db_cluster_version      = string<br/>    category                = string<br/>    db_cluster_class        = string<br/>    db_cluster_network_type = string<br/>    db_node_group_count     = string<br/>    payment_type            = string<br/>    db_node_storage         = string<br/>    storage_type            = string<br/>    db_cluster_description  = string<br/>  })</pre> | <pre>{<br/>  "category": "Basic",<br/>  "db_cluster_class": "S8",<br/>  "db_cluster_description": "ck",<br/>  "db_cluster_network_type": "vpc",<br/>  "db_cluster_version": "23.8",<br/>  "db_node_group_count": "1",<br/>  "db_node_storage": "100",<br/>  "payment_type": "PayAsYouGo",<br/>  "storage_type": "cloud_essd"<br/>}</pre> | no |
| <a name="input_custom_ecs_command_script"></a> [custom\_ecs\_command\_script](#input\_custom\_ecs\_command\_script) | Custom ECS command script for HTAP installation. If not provided, the default script will be used. | `string` | `null` | no |
| <a name="input_db_database_config"></a> [db\_database\_config](#input\_db\_database\_config) | The parameters of database. The attributes 'character\_set', 'name', 'description' are required. | <pre>object({<br/>    character_set = string<br/>    name          = string<br/>    description   = string<br/>  })</pre> | <pre>{<br/>  "character_set": "utf8mb4",<br/>  "description": "tpcc",<br/>  "name": "tpcc"<br/>}</pre> | no |
| <a name="input_ecs_command_config"></a> [ecs\_command\_config](#input\_ecs\_command\_config) | The parameters of ECS command. The attributes 'name', 'description', 'type', 'timeout', 'working\_dir' are required. | <pre>object({<br/>    name        = string<br/>    description = string<br/>    type        = string<br/>    timeout     = number<br/>    working_dir = string<br/>  })</pre> | <pre>{<br/>  "description": "commond_install_description",<br/>  "name": "commond_install",<br/>  "timeout": 1800,<br/>  "type": "RunShellScript",<br/>  "working_dir": "/root"<br/>}</pre> | no |
| <a name="input_ecs_instance_config"></a> [ecs\_instance\_config](#input\_ecs\_instance\_config) | The parameters of ECS instance. The attributes 'instance\_type', 'password', 'system\_disk\_category', 'internet\_max\_bandwidth\_out', 'image\_id', and 'instance\_name' are required. | <pre>object({<br/>    instance_type              = string<br/>    password                   = string<br/>    system_disk_category       = string<br/>    internet_max_bandwidth_out = number<br/>    image_id                   = string<br/>    instance_name              = string<br/>  })</pre> | <pre>{<br/>  "image_id": null,<br/>  "instance_name": "ecs-instance",<br/>  "instance_type": "ecs.e-c1m2.large",<br/>  "internet_max_bandwidth_out": 5,<br/>  "password": null,<br/>  "system_disk_category": "cloud_essd"<br/>}</pre> | no |
| <a name="input_ecs_invocation_config"></a> [ecs\_invocation\_config](#input\_ecs\_invocation\_config) | The parameters of ECS invocation. The attribute 'create\_timeout' is required. | <pre>object({<br/>    create_timeout = string<br/>  })</pre> | <pre>{<br/>  "create_timeout": "30m"<br/>}</pre> | no |
| <a name="input_rds_db_account_config"></a> [rds\_db\_account\_config](#input\_rds\_db\_account\_config) | The parameters of RDS database account. The attributes 'account\_name', 'account\_password', 'account\_type' are required. | <pre>object({<br/>    account_name     = string<br/>    account_password = string<br/>    account_type     = string<br/>  })</pre> | <pre>{<br/>  "account_name": "rds_user",<br/>  "account_password": null,<br/>  "account_type": "Super"<br/>}</pre> | no |
| <a name="input_rds_instance_config"></a> [rds\_instance\_config](#input\_rds\_instance\_config) | The parameters of RDS instance. The attributes 'engine', 'engine\_version', 'instance\_storage', 'category', 'db\_instance\_storage\_type', 'instance\_charge\_type', 'instance\_type', and 'instance\_name' are required. | <pre>object({<br/>    engine                   = string<br/>    engine_version           = string<br/>    instance_storage         = number<br/>    category                 = string<br/>    db_instance_storage_type = string<br/>    instance_charge_type     = string<br/>    security_ips             = list(string)<br/>    instance_type            = string<br/>    instance_name            = string<br/>  })</pre> | <pre>{<br/>  "category": "HighAvailability",<br/>  "db_instance_storage_type": "cloud_essd",<br/>  "engine": "MySQL",<br/>  "engine_version": "8.0",<br/>  "instance_charge_type": "PostPaid",<br/>  "instance_name": "rdsmysql",<br/>  "instance_storage": 100,<br/>  "instance_type": "mysql.x4.xlarge.2c",<br/>  "security_ips": [<br/>    "0.0.0.0/0"<br/>  ]<br/>}</pre> | no |
| <a name="input_security_group_config"></a> [security\_group\_config](#input\_security\_group\_config) | The parameters of security group. | <pre>object({<br/>    security_group_name = optional(string, "security-group")<br/>    description         = optional(string, "Security group for RDS ClickHouse HTAP solution")<br/>  })</pre> | `{}` | no |
| <a name="input_vpc_config"></a> [vpc\_config](#input\_vpc\_config) | The parameters of VPC. The attributes 'cidr\_block' and 'vpc\_name' are required. | <pre>object({<br/>    cidr_block = string<br/>    vpc_name   = string<br/>  })</pre> | n/a | yes |
| <a name="input_vswitch_config"></a> [vswitch\_config](#input\_vswitch\_config) | The parameters of VSwitch. The attributes 'cidr\_block', 'zone\_id', and 'vswitch\_name' are required. | <pre>object({<br/>    cidr_block   = string<br/>    zone_id      = string<br/>    vswitch_name = string<br/>  })</pre> | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_clickhouse_account_name"></a> [clickhouse\_account\_name](#output\_clickhouse\_account\_name) | The name of the ClickHouse account |
| <a name="output_clickhouse_account_password"></a> [clickhouse\_account\_password](#output\_clickhouse\_account\_password) | The password of the ClickHouse account |
| <a name="output_clickhouse_cluster_connection_string"></a> [clickhouse\_cluster\_connection\_string](#output\_clickhouse\_cluster\_connection\_string) | The connection string of the ClickHouse cluster |
| <a name="output_clickhouse_cluster_id"></a> [clickhouse\_cluster\_id](#output\_clickhouse\_cluster\_id) | The ID of the ClickHouse cluster |
| <a name="output_clickhouse_cluster_port"></a> [clickhouse\_cluster\_port](#output\_clickhouse\_cluster\_port) | The port of the ClickHouse cluster |
| <a name="output_ecs_command_id"></a> [ecs\_command\_id](#output\_ecs\_command\_id) | The ID of the ECS command |
| <a name="output_ecs_instance_id"></a> [ecs\_instance\_id](#output\_ecs\_instance\_id) | The ID of the ECS instance |
| <a name="output_ecs_instance_password"></a> [ecs\_instance\_password](#output\_ecs\_instance\_password) | The password of the ECS instance |
| <a name="output_ecs_instance_private_ip"></a> [ecs\_instance\_private\_ip](#output\_ecs\_instance\_private\_ip) | The private IP address of the ECS instance |
| <a name="output_ecs_instance_public_ip"></a> [ecs\_instance\_public\_ip](#output\_ecs\_instance\_public\_ip) | The public IP address of the ECS instance |
| <a name="output_ecs_invocation_id"></a> [ecs\_invocation\_id](#output\_ecs\_invocation\_id) | The ID of the ECS command invocation |
| <a name="output_rds_account_name"></a> [rds\_account\_name](#output\_rds\_account\_name) | The name of the RDS database account |
| <a name="output_rds_account_password"></a> [rds\_account\_password](#output\_rds\_account\_password) | The password of the RDS database account |
| <a name="output_rds_database_name"></a> [rds\_database\_name](#output\_rds\_database\_name) | The name of the RDS database |
| <a name="output_rds_instance_connection_string"></a> [rds\_instance\_connection\_string](#output\_rds\_instance\_connection\_string) | The connection string of the RDS instance |
| <a name="output_rds_instance_id"></a> [rds\_instance\_id](#output\_rds\_instance\_id) | The ID of the RDS instance |
| <a name="output_rds_instance_port"></a> [rds\_instance\_port](#output\_rds\_instance\_port) | The port of the RDS instance |
| <a name="output_security_group_id"></a> [security\_group\_id](#output\_security\_group\_id) | The ID of the security group |
| <a name="output_vpc_cidr_block"></a> [vpc\_cidr\_block](#output\_vpc\_cidr\_block) | The CIDR block of the VPC |
| <a name="output_vpc_id"></a> [vpc\_id](#output\_vpc\_id) | The ID of the VPC |
| <a name="output_vswitch_cidr_block"></a> [vswitch\_cidr\_block](#output\_vswitch\_cidr\_block) | The CIDR block of the VSwitch |
| <a name="output_vswitch_id"></a> [vswitch\_id](#output\_vswitch\_id) | The ID of the VSwitch |
| <a name="output_vswitch_zone_id"></a> [vswitch\_zone\_id](#output\_vswitch\_zone\_id) | The zone ID of the VSwitch |
<!-- END_TF_DOCS -->

## Submit Issues

If you have any problems when using this module, please opening
a [provider issue](https://github.com/aliyun/terraform-provider-alicloud/issues/new) and let us know.

**Note:** There does not recommend opening an issue on this repo.

## Authors

Created and maintained by Alibaba Cloud Terraform Team(terraform@alibabacloud.com).

## License

MIT Licensed. See LICENSE for full details.

## Reference

* [Terraform-Provider-Alicloud Github](https://github.com/aliyun/terraform-provider-alicloud)
* [Terraform-Provider-Alicloud Release](https://releases.hashicorp.com/terraform-provider-alicloud/)
* [Terraform-Provider-Alicloud Docs](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs)