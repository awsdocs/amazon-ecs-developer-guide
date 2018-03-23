# ecs\-cli ps<a name="cmd-ecs-cli-ps"></a>

## Description<a name="cmd-ecs-cli-ps-description"></a>

Lists all running containers in your ECS cluster\.

The IP address displayed by the Amazon ECS CLI depends heavily upon how you have configured your task and cluster:
+ For tasks using the EC2 launch type without task networking ‐ the IP address shown is the public IP address of the Amazon EC2 instance running your task, or the instances private IP if it lacks a public IP address\.
+ For tasks using the EC2 launch type with task networking ‐ the ECS CLI only shows a private IP address obtained from the network interfaces section of the Describe Task output for the task\.
+ For tasks using the Fargate launch type ‐ the Amazon ECS CLI returns the public IP assigned to the elastic network instance attached to the Fargate task\. If the elastic network instance lacks a public IP, then the Amazon ECS CLI falls back to the private IP obtained from the network interfaces section of the Describe Task output\.

## Syntax<a name="cmd-ecs-cli-ps-syntax"></a>

**ecs\-cli ps \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-ps-options"></a>


| Name | Description | 
| --- | --- | 
|  `--cluster, -c cluster_name`  |  Specifies the ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-ps-examples"></a>

### Example<a name="cmd-ecs-cli-ps-example-1"></a>

This example shows the containers that are running in the cluster\.

```
ecs-cli ps
```

Output:

```
Name                                            State    Ports                     TaskDefinition
595deba7-16a1-4aaf-9b27-e152eba03ccc/wordpress  RUNNING  52.33.62.24:80->80/tcp    ecscompose-hello-world:3
595deba7-16a1-4aaf-9b27-e152eba03ccc/mysql      RUNNING                            ecscompose-hello-world:3
7fc0a2a4-9202-47d2-8b06-4463286b63de/mysql      RUNNING                            ecscompose-hello-world:3
7fc0a2a4-9202-47d2-8b06-4463286b63de/wordpress  RUNNING  52.32.232.166:80->80/tcp  ecscompose-hello-world:3
```