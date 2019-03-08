# ecs\-cli check\-attributes<a name="cmd-ecs-cli-check-attributes"></a>

Checks if a given list of container instances can run a given task definition by checking their attributes\. Outputs attributes that are required by the task definition but not present on the container instances\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-check-attributes-syntax"></a>

**ecs\-cli check\-attributes \[\-\-task\-def *task\_definition*\] \[\-\-container\-instances *value*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-check-attributes-options"></a>


| Name | Description | 
| --- | --- | 
|  `--task-def task_definition`  |  Specifies the name or full Amazon Resource Name \(ARN\) of the ECS task definition associated with the task ID\. This is only needed if the task has been stopped\. Type: String Required: No  | 
|  `--container-instances value`  |  A list of container instance IDs or full ARN entries to check if all required attributes are available for the Task Definition to RunTask\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-check-attributes-examples"></a>

### Example<a name="cmd-ecs-cli-check-attributes-example-1"></a>

This example checks multiple container instances and verifies that they contain the attributes necessary to successfully run the specified task definition\.

```
ecs-cli check-attributes --container-instances 28c5abd2-360e-41a0-81d8-0afca2d08d9b,45510138-f24f-47c6-a418-71c46dd51f88 --cluster default --region us-east-2 --task-def fluentd-test
```

Output:

```
Container Instance                    Missing Attributes
28c5abd2-360e-41a0-81d8-0afca2d08d9b  com.amazonaws.ecs.capability.logging-driver.fluentd
45510138-f24f-47c6-a418-71c46dd51f88  None
```