# terraform-aws-config

This is a terraform module for enabling and configuring AWS Config.

## Description

AWS Config rules will be set up in the account to check on the following things:

* Resource Tagging
   * Checks if the resources in your account are tagged properly
* Access Key Rotation
* RDS Instances without backups enabled
* EC2 Instances with Public IP addresses enabled
* ElasticSearch outside a VPC
* Logging enabled for all LoadBalancers
* Root User with access and secret key
* RDS Instances with Public access
* S3 Buckets configured as a static WebServer
* IAM Users with Console Login enabled

## Usage

The following default values are set:

* accessKeyRotate_maxAccessKeyAge               = 30
* dbInstanceBackupEnabled_RetentionPeriod       = 30
* dbInstanceBackupEnabled_PreferredBackupWindow = "22:00-24:00"
* dbInstanceBackupEnabled_CheckReadReplicas     = true
* elbLoggingEnabled_s3BucketNames               = "backup"
* lambda_timeout                                = 30

```hcl
module "aws-config" {
  source = "git::https://github.com/zoitech/terraform-aws-config.git?ref=2.0.0"
  accessKeyRotate_maxAccessKeyAge               = 180
  dbInstanceBackupEnabled_RetentionPeriod       = 90
  dbInstanceBackupEnabled_PreferredBackupWindow = "23:00-01:00"
  dbInstanceBackupEnabled_CheckReadReplicas     = true
  elbLoggingEnabled_s3BucketNames               = "backup"
  lambda_timeout                                = 60
}
```## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_cloudwatch_log_group.s3_webserver_buckets_cloudwatch](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_log_group) | resource |
| [aws_config_organization_custom_rule.iam_console_login](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_custom_rule) | resource |
| [aws_config_organization_custom_rule.s3WebserverBuckets](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_custom_rule) | resource |
| [aws_config_organization_managed_rule.accessKeyRotated](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.dbInstanceBackupEnabled](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.ec2InstanceNoPublicIp](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.elasticsearchInVpcOnly](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.elbLoggingEnabled](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.iamRootAccessKeyCheck](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.rdsInstancePublicAccessCheck](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_config_organization_managed_rule.resourcesTagged](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/config_organization_managed_rule) | resource |
| [aws_iam_policy.lambda_logging](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_policy.lambda_put_config_evaluations](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.aws_config_recorder](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.iam_console_login_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.s3_webserver_buckets_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy_attachment.iam_console_login_cloudwatch](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.iam_console_login_config](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.iam_console_login_iam_read_only](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.s3_webserver_buckets_attachment_config_putevaluation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.s3_webserver_buckets_attachment_s3readonly](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.s3_webserver_buckets_cloudwatch](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_lambda_function.iam_console_login](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function) | resource |
| [aws_lambda_function.s3_webserver_buckets](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function) | resource |
| [aws_lambda_permission.s3_webserver_buckets_config_permissions](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_permission) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_accessKeyRotated_maxAccessKeyAge"></a> [accessKeyRotated\_maxAccessKeyAge](#input\_accessKeyRotated\_maxAccessKeyAge) | Every Access Key will be defined as Non-Compliant after exceeding the number of days defined in this variable | `number` | `30` | no |
| <a name="input_dbInstanceBackupEnabled_CheckReadReplicas"></a> [dbInstanceBackupEnabled\_CheckReadReplicas](#input\_dbInstanceBackupEnabled\_CheckReadReplicas) | Defines if AWS Config should Check if the RDS instance has backups enabled for the ReadReplicas | `bool` | `true` | no |
| <a name="input_dbInstanceBackupEnabled_PreferredBackupWindow"></a> [dbInstanceBackupEnabled\_PreferredBackupWindow](#input\_dbInstanceBackupEnabled\_PreferredBackupWindow) | The format is hh24:min-hh24:min. Example: 23:00-02:00 | `string` | `"22:00-24:00"` | no |
| <a name="input_dbInstanceBackupEnabled_RetentionPeriod"></a> [dbInstanceBackupEnabled\_RetentionPeriod](#input\_dbInstanceBackupEnabled\_RetentionPeriod) | The retention period in days for the RDS Databases to check | `number` | `30` | no |
| <a name="input_elbLoggingEnabled_s3BucketNames"></a> [elbLoggingEnabled\_s3BucketNames](#input\_elbLoggingEnabled\_s3BucketNames) | Comma separated list of S3 bucket names for ELB to deliver the log files | `string` | `"backup"` | no |
| <a name="input_lambda_timeout"></a> [lambda\_timeout](#input\_lambda\_timeout) | The timeout for the custom lambda scripts which define more custom AWS Config rules | `number` | `30` | no |
| <a name="input_required_tags"></a> [required\_tags](#input\_required\_tags) | A map of the required tag keys and/or values to evaluate | `map(string)` | <pre>{<br>  "tag1Key": "owner"<br>}</pre> | no |

## Outputs

No outputs.
