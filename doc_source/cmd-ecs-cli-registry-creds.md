# ecs\-cli registry\-creds<a name="cmd-ecs-cli-registry-creds"></a>

Facilitates the creation and use of private registry credentials within Amazon ECS\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-registry-creds-syntax"></a>

**ecs\-cli registry\-creds \[\-\-region *region*\] \[\-\-cluster\-config *cluster\_config\_name*\] \[\-\-ecs\-profile *ecs\_profile*\] \[\-\-aws\-profile *aws\_profile*\] \[\-\-help\] \[*subcommand*\] \[*arguments*\] \[\-\-help\]**

## Options<a name="cmd-ecs-cli-registry-creds-options"></a>


| Name | Description | 
| --- | --- | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-registry-creds-subcommands"></a>

The ecs\-cli registry\-creds command supports the following subcommands\. Each of these subcommands has their own flags associated with them, which can be displayed using the `--help` flag\.

up  
Generates AWS Secrets Manager secrets and an IAM task execution role for use in an Amazon ECS task definition\. For more information, see [ecs\-cli registry\-creds up](cmd-ecs-cli-registry-creds-up.md)\.

help  
Shows the help text for the specified command\.