# ecs\-cli scale<a name="cmd-ecs-cli-scale"></a>

Modifies the number of container instances in your cluster\. This command changes the desired and maximum instance count in the Auto Scaling group created by the ecs\-cli up command\. You can use this command to scale out \(increase the number of instances\) or scale in \(decrease the number of instances\) your cluster\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the Amazon ECS CLI\. To manage tasks, services, and container instances that weren't created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-scale-syntax"></a>

**ecs\-cli scale \-\-capability\-iam \-\-size *n* \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-scale-options"></a>


| Name | Description | 
| --- | --- | 
|  `--capability-iam`  |  Acknowledges that this command may create IAM resources\. Required: Yes  | 
|  `--size n`  |  Specifies the number of instances to maintain in your cluster\. Type: Integer Required: Yes  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-scale-examples"></a>

### Example<a name="cmd-ecs-cli-scale-example-1"></a>

This example scales the current cluster to two container instances\.

```
ecs-cli scale --size 2 --capability-iam
```

Output:

```
INFO[0001] Waiting for your cluster resources to be updated
INFO[0001] Cloudformation stack status                   stackStatus=UPDATE_IN_PROGRESS
```