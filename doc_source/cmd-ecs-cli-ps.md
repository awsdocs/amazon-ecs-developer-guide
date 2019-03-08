# ecs\-cli ps<a name="cmd-ecs-cli-ps"></a>

Lists all running containers in your Amazon ECS cluster\.

The IP address displayed by the Amazon ECS CLI depends heavily upon how you have configured your task and cluster:
+ For tasks using the EC2 launch type without task networking, the IP address shown is the public IP address of the Amazon EC2 instance running your task, or the instance private IP address if it lacks a public IP address\.
+ For tasks using the EC2 launch type with task networking, the ECS CLI only shows a private IP address obtained from the network interfaces section of the Describe Task output for the task\.
+ For tasks using the Fargate launch type, the Amazon ECS CLI returns the public IP address assigned to the elastic network instance attached to the Fargate task\. If the elastic network instance lacks a public IP address, then the Amazon ECS CLI falls back to the private IP address obtained from the network interfaces section of the Describe Task output\.

## Syntax<a name="cmd-ecs-cli-ps-syntax"></a>

**ecs\-cli ps \[\-\-desired\-status *status*\] \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-ps-options"></a>


| Name | Description | 
| --- | --- | 
|  `--desired-status status`  |  The container desired status to filter the container list results with\. Required: No Valid values: `RUNNING` \| `STOPPED`  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-ps-examples"></a>

### Example<a name="cmd-ecs-cli-ps-example-1"></a>

This example shows the containers that are running in the cluster\.

```
ecs-cli ps
```

Output:

```
Name                                             State    Ports                      TaskDefinition       Health
afd7f8a0-3813-4e1a-9d9e-ca7e9d1fcfbb/wordpress   RUNNING  36.253.177.221:80->80/tcp  compose3:7           HEALTHY
dca67e02-68ca-4507-b194-a47239b5e7a9/wordpress   RUNNING  37.234.146.14:80->80/tcp   healthcheck:3        UNKNOWN
dca67e02-68ca-4507-b194-a47239b5e7a9/redis       RUNNING                             healthcheck:3        HEALTHY
feb6e10e-3385-4c9b-a6cb-787cc8e90dda/sample-app  RUNNING  54.229.211.206:80->80/tcp  tutorial-task-def:1  UNKNOWN
```