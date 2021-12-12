# aws_rds_database

This module can create RDS Database and RDS Read Replica<br>


# Inputs

| Name               | Description                                                                             | Type   | Default         | Required |
|------              |-------------                                                                             |:------:|:--------------:|:--------:|
| identifier         | The name of the RDS instance                                                             | string | n/a            | yes    |
| allocated\_storage | The allocated storage in gigabytes                                                       | string | n/a            | yes    |
| storage\_type      | 'standard' (magnetic), 'gp2' (general purpose SSD), or 'io1' (provisioned IOPS)          | string | gp2            | yes    |
| storage\_encrypted | Specifies whether the DB instance is encrypted                                           | bool   | false          | yes    |
| kms\_key\_id       | The ARN for the KMS encryption key                                                       | string | n/a            | no     |
| is\_replica        | Specifies whether the instance to be created is a replica or not                         | string | false          | yes    |
| replicate\_source\_db | Specifies that this resource is a Replicate database, and to use this value as the source database. This correlates to the identifier of another Amazon RDS Database to replicate                                                          | string | n/a            | no     |
| snapshot\_identifier | Specifies whether or not to create this database from a snapshot. This correlates to the snapshot ID you'd find in the RDS console | string | n/a | no |
| license\_model     | License model information for this DB instance. Optional, but required for some DB engines, i.e. Oracle SE1 | string| n/a | no |
| iam\_database\_authentication\_enabled | Specifies whether or mappings of IAM accounts to database accounts is enabled | bool | false | yes |
| engine              | The RDS database engine to use                                   | string | n/a | yes |
| engine\_version     | The engine version to use                                        | string | n/a | yes |
| final_snapshot_identifier | The name of your final DB snapshot when this DB instance is deleted, otherwise not create | string | false | no |
| instance\_class     | The instance type of the RDS instance                            | string | n/a | yes |
| name                | The DB name to create in this RDS Insatnce. If omitted, no database is created initially | string | n/a | no |
| username            | Database Master Username                                         | string | n/a | yes |
| password            | Database Master Password                                         | string | n/a | yes |
| port                | The port on which the DB accepts connections                     | string | n/a | yes |
| vpc\_security\_group\_ids | List of VPC security groups to associate to RDS               | list   | []  | yes |
| db\_subnet\_group\_name   | Name of DB subnet group. DB instance will be created in the VPC associated with the DB subnet group. If unspecified, will be created in the default VPC | string | n/a | no |
| parameter\_group\_name  | Name of the DB parameter group to associate. Setting this automatically disables parameter_group creation| string | n/a | no |
| option\_group\_name     | Name of the DB option group to associate. Setting this automatically disables option_group creation      | string | n/a | no |
| availability\_zone      | The Availability Zone of the RDS instance                                                                | string | n/a | no |
| multi\_az               | Specifies if the RDS instance is multi-AZ                                                               | bool | false | yes |
| iops                    | The amount of provisioned IOPS. Setting this implies a storage_type of 'io1' only                       | number| 0 | no |
| publicly\_accessible    | Bool to control if instance is publicly accessible true/false                                           | bool | false | yes|
| monitoring\_interval    | The interval, in seconds, between points when Enhanced Monitoring metrics are collected for the DB instance. To disable collecting Enhanced Monitoring metrics, specify 0. Valid Values: 0, 1, 5, 10, 15, 30, 60. | number | 0 | yes |
| monitoring\_role\_arn   | The ARN for the IAM role that permits RDS to send enhanced monitoring metrics to CloudWatch Logs. Must be specified if monitoring_interval is non-zero | string | n/a | no |
| monitoring\_role\_name   | Name of the IAM role which will be created when create_monitoring_role is enabled | string | rds-monitoring-role | no |
| create\_monitoring\_role | Create IAM role with a defined name that permits RDS to send enhanced monitoring metrics to CloudWatch Logs | bool | false | no |
| allow\_major\_version\_upgrade| Indicates that major version upgrades are allowed. Changing this parameter does not result in an outage and the change is asynchronously applied as soon as possible | bool | false | yes |
|auto\_minor\_version\_upgrade |Indicates that minor engine upgrades will be applied automatically to the DB instance during the maintenance window| bool | true | yes |
| apply\_immediately  | Specifies whether any database modifications are applied immediately, or during the next maintenance window | bool | false | yes |
| maintenance\_window | The window to perform maintenance in. Syntax: 'ddd:hh24:mi-ddd:hh24:mi'. Eg: 'Mon:00:00-Mon:03:00' | string | n/a | yes |
| skip\_final\_snapshot|Determines whether a final DB snapshot is created before the DB instance is deleted. If true is specified, no DBSnapshot is created. If false is specified, a DB snapshot is created before the DB instance is deleted, using the value from final_snapshot_identifier | bool | true | yes |
| copy\_tags\_to\_snapshot | On delete, copy all Instance tags to the final snapshot (if final_snapshot_identifier is specified) | bool | false | no |
| backup\_retention\_period| The days to retain backups for RDS, 0 to disable backup. | number | 1 | yes |
| backup\_window     | The daily time range (in UTC) during which automated backups are created if they are enabled. Example: '09:46-10:16'. Must not overlap with maintenance_window| string | n/a | yes |
| tags | A mapping of tags to assign to all resources | map | {} | no |
| subnet\_ids | A list of VPC subnet IDs for DB Subnet Group| list | n/a | yes |
| family      | The family of the DB parameter group        | string | n/a | no |
| parameters  | A list of DB parameters (map) to apply      | map | n/a | no |
| option\_group\_description | The description of the option group | string | n/a| no |
| major\_engine\_version   |Specifies the major version of the engine that this option group should be associated with | string | n/a | yes |
| options | A list of Options to apply | list | [] | no |
| create\_db\_subnet\_group | Whether to create a database subnet group | bool | true | yes |
| create\_db\_parameter\_group |Whether to create a database parameter group | bool | true | yes |
| create\_db\_option\_group    |Whether to create a database option group  | bool | true | yes |
| create\_db\_instance         |Whether to create a database instance | bool | true | yes |
| timezone | Time zone of the DB instance. timezone is currently only supported by Microsoft SQL Server. The timezone can only be set on creation. See MSSQL User Guide for more information | string | n/a | no |
| character\_set\_name   | The character set name to use for DB encoding in Oracle instances. This can't be changed. See Oracle Character Sets Supported in Amazon RDS for more information | string | n/a | no |
| enabled_cloudwatch_logs_exports | A list of all enabled cloudwatch log export targets | list | [] | no |




## Outputs

| Name                          | Description |
|-------------------------------|-------------|
| this\_db\_instance\_address   | The address of the RDS instance  |
| this\_db\_instance\_arn       | The ARN of the RDS instance      |
| this\_db\_instance\_availability\_zone | The availability zone of the RDS instance |
| this\_db\_instance\_endpoint  | The connection endpoint |
| this\_db\_instance\_hosted\_zone\_id | The canonical hosted zone ID of the DB instance (to be used in a Route 53 Alias record) |
| this\_db\_instance\_id        | The RDS instance ID |
| this\_db\_instance\_resource\_id | The RDS Resource ID of this instance |
| this\_db\_instance\_status       | The RDS instance status |
| this\_db\_instance\_name         | The database name |
| this\_db\_instance\_username     | The master username for the database |
| this\_db\_instance\_password     | The database password (this password may be old, because Terraform doesn't track it after initial creation) |
| this\_db\_instance\_port         | The database port |
| this\_db\_subnet\_group\_id      | The db subnet group name |
| this\_db\_subnet\_group\_arn     | The ARN of the db subnet group |
| this\_db\_parameter\_group\_id   | The db parameter group id |
| this\_db\_parameter\_group\_arn  | The ARN of the db parameter group |
| this\_db\_option\_group\_id      | The DB option group id |
| this\_db\_option\_group\_arn     | The ARN of the db option group |


Example of use to create Database:
```
module "db" {
  source = "../../../../../../../modules/aws_rds_database/"

  identifier            = "opsguru-${var.account_name}"
  engine                = "${var.rds_engine}"
  engine_version        = "${var.rds_engine_version}"
  instance_class        = "${var.db_instance_type}"
  multi_az              = "${var.db_multi_az}"
  allocated_storage     = "${var.db_allocated_storage}"
  storage_encrypted     = true
  copy_tags_to_snapshot = true
  kms_key_id            = "${data.terraform_remote_state.kms.kms_key_rds_id}"

  name     = "opsguru-db"                                    # Name of Database to create in RDS Instance
  username = "root"
  password = "${data.aws_ssm_parameter.rds_password.value}"
  port     = "${var.rds_port}"

  monitoring_interval             = "${var.db_monitoring_interval}"
  create_monitoring_role          = "${var.db_create_monitoring_role}"
  monitoring_role_name            = "opsguru-rds-monitoring-role"
  enabled_cloudwatch_logs_exports = "${var.db_cw_logs_exports}"

  major_engine_version       = "${var.rds_engine_version}"
  auto_minor_version_upgrade = "${var.db_auto_minor_version_upgrade}"
  vpc_security_group_ids     = ["${aws_security_group.main.id}"]
  maintenance_window         = "${var.rds_maintenance_window}"
  backup_window              = "${var.rds_backup_window}"
  backup_retention_period    = "${var.db_backup_retention_period}"
  tags                       = "${local.tags}"
  subnet_ids                 = ["${data.terraform_remote_state.vpc.private_subnets}"]

  final_snapshot_identifier = "opsguru-${var.account_name}"

  create_db_parameter_group = false

  parameter_group_name = "${var.rds_parameter_group_name}"
}

```


Example of use to create Read Replica:
```
module "db" {
  source = "../../../../../../../modules/aws_rds_database/"

  is_replica            = "true"
  identifier            = "opsguru-replica-${var.account_name}"
  replicate_source_db   = "opsguru-${var.account_name}"
  engine                = "${var.rds_engine}"
  engine_version        = "${var.rds_engine_version}"
  instance_class        = "${var.db_instance_type}"
  multi_az              = "${var.db_multi_az}"
  allocated_storage     = "${var.db_allocated_storage}"
  storage_encrypted     = true
  copy_tags_to_snapshot = true
  kms_key_id            = "${data.terraform_remote_state.kms.kms_key_rds_id}"

  name     = "opsguru-db"      # Name of Database to create in RDS Instance
  username = ""                # Not used in Read Replica
  password = ""                # Not used in Read Replica
  port     = "${var.rds_port}"

  monitoring_interval             = "${var.db_monitoring_interval}"
  create_monitoring_role          = "${var.db_create_monitoring_role}"
  monitoring_role_name            = "opsguru-replica-rds-monitoring-role"
  enabled_cloudwatch_logs_exports = "${var.db_cw_logs_exports}"

  major_engine_version       = "${var.rds_engine_version}"
  auto_minor_version_upgrade = "${var.db_auto_minor_version_upgrade}"
  vpc_security_group_ids     = ["${aws_security_group.main.id}"]
  maintenance_window         = ""                                                     # Not used in Read Replica
  backup_window              = ""                                                     # Not used in Read Replica
  backup_retention_period    = 0                                                      # Not used in Read Replica
  tags                       = "${local.tags}"
  subnet_ids                 = ["${data.terraform_remote_state.vpc.private_subnets}"]

  final_snapshot_identifier = "opsguru-replica-${var.account_name}"

  create_db_parameter_group = false

  parameter_group_name = "${var.rds_parameter_group_name}"
}
```
