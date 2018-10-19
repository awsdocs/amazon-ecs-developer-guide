# ecs\-cli down<a name="cmd-ecs-cli-down"></a>

Deletes the AWS CloudFormation stack that was created by ecs\-cli up and the associated resources\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the Amazon ECS CLI\. To manage tasks, services, and container instances that were not created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

The ecs\-cli down command attempts to delete the cluster specified in `~/.ecs/config`\. However, if there are any active services \(even with a desired count of 0\) or registered container instances in your cluster that were not created by ecs\-cli up, the cluster is not deleted and the services and pre\-existing container instances remain active\. This might happen, for example, if you used an existing ECS cluster with registered container instances, such as the default cluster\.

If you have remaining services or container instances in your cluster that you would like to remove, you can follow the procedures in [Cleaning Up your Amazon ECS Resources](ECS_CleaningUp.md) to remove them and then delete your cluster\.

**Important**  
Some features described may only be available with the latest version of the Amazon ECS CLI\. To obtain the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-down-syntax"></a>

**ecs\-cli down \[\-\-force\] \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-down-options"></a>


| Name | Description | 
| --- | --- | 
|  `--force, -f`  |  Acknowledges that this command permanently deletes resources and bypasses the confirmation prompt\. Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-down-examples"></a>

### Example 1<a name="cmd-ecs-cli-down-example-1"></a>

This example deletes a cluster that contains resources\.

```
ecs-cli down --cluster ecs-cli-fargate-demo --force
```

Output:

```
INFO[0001] Waiting for your cluster resources to be deleted
INFO[0001] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0062] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0123] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0154] Deleted cluster
```

### Example 2<a name="cmd-ecs-cli-down-example-2"></a>

This example deletes an empty cluster\.

```
ecs-cli down --cluster ecs-cli-empty-demo --force
```

Output:

```
INFO[0002] No CloudFormation stack found for cluster 'ecs-cli-empty-demo'. 
INFO[0003] Deleted cluster                               cluster=ecs-cli-empty-demo
```