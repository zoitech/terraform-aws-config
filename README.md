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
* dbInstanceBackupEnabled_CheckReadReplicas     = True
* elbLoggingEnabled_s3BucketNames               = "backup"
* lambda_timeout                                = 30

```hcl
module "aws-config" {
  source = "git::https://github.com/zoitech/terraform-aws-config.git?ref=v1.0.0"
  accessKeyRotate_maxAccessKeyAge               = 180
  dbInstanceBackupEnabled_RetentionPeriod       = 90
  dbInstanceBackupEnabled_PreferredBackupWindow = "23:00-01:00"
  dbInstanceBackupEnabled_CheckReadReplicas     = True
  elbLoggingEnabled_s3BucketNames               = "backup"
  lambda_timeout                                = 60
}
```