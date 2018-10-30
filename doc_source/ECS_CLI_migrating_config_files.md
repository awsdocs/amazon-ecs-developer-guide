# Migrating Configuration Files<a name="ECS_CLI_migrating_config_files"></a>

The process of configuring the Amazon ECS CLI has changed significantly in the latest version \(v1\.0\.0\) to allow the addition of new features\. A migration command has been introduced that converts an older \(v0\.6\.6 and older\) configuration file to the current format\. The old configuration files are deprecated, so we recommend converting your configuration to the newest format to take advantage of the new features\. The configuration\-related changes and new features introduced in v1\.0\.0 in the new YAML formatted configuration files include:
+ Splitting up of credential and cluster\-related configuration information into two separate files\. Credential information is stored in `~/.ecs/credentials` and cluster configuration information is stored in `~/.ecs/config`\.
+ The configuration files are formatted in YAML\.
+ Support for storing multiple named configurations\.
+ Deprecation of the field `compose-service-name-prefix` \(name used for creating a service `<compose_service_name_prefix> + <project_name>`\)\. This field can still be configured\. However, if it is not configured, there is no longer a default value assigned\. For Amazon ECS CLI v0\.6\.6 and earlier, the default was `ecscompose-service-`\.
+ Removal of the field `compose-project-name-prefix` \(name used for creating a task definition `<compose_project_name_prefix> + <project_name>`\)\. Amazon ECS CLI v1\.0\.0 and later can still read old configuration files; so if this field is present then it is still read and used\. However, configuring this field is not supported in v1\.0\.0\+ with the `ecs-cli configure` command, and if the field is manually added to a v1\.0\.0\+ configuration file it causes the Amazon ECS CLI to throw an error\.
+ The field `cfn-stack-name-prefix` \(name used for creating CFN stacks `<cfn_stack_name_prefix> + <cluster_name>`\) has been changed to `cfn-stack-name`\. Instead of specifying a prefix, the exact name of a CloudFormation template can be configured\.
+ Amazon ECS CLI v0\.6\.6 and earlier allowed configuring credentials using a named AWS profile from the `~/.aws/credentials` file on your system\. This functionality has been removed\. However, a new flag, `--aws-profile`, has been added which allows the referencing of an AWS profile inline in all commands that require credentials\.

**Note**  
The `--project-name` flag can be used to set the project name\.

## Migrating Older Configuration Files to the v1\.0\.0\+ Format<a name="ECS_CLI_migrating_config_files_notes"></a>

While all versions of the Amazon ECS CLI support reading from the older configuration file format, upgrading to the new format is required to take advantage of some new features, for example using multiple named cluster profiles\. Migrating your legacy configuration file to the new format is easy with the `ecs-cli configure migrate` command\. The command takes the configuration information stored in the old format in `~/.ecs/config` and converts it to a pair of files in the new format, overwriting your old configuration file in the process\.

When running the `ecs-cli configure migrate` command there is a warning message displayed with the old configuration file, and a preview of the new configuration files\. User confirmation is required before the migration proceeds\. If the `--force` flag is used, then the warning message is not displayed, and the migration proceeds without any confirmation\. If `cfn-stack-name-prefix` is used in the legacy file, then `cfn-stack-name` is stored in the new file as `<cfn_stack_name_prefix> + <cluster_name>`\.

For more information, see the [Amazon ECS Command Line Reference](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html) in the *Amazon Elastic Container Service Developer Guide*\.