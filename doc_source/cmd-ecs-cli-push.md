# ecs\-cli push<a name="cmd-ecs-cli-push"></a>

## Description<a name="cmd-ecs-cli-push-description"></a>

Pushes an image to an Amazon ECR repository\.

**Important**  
Some features described may only be available with the latest version of the ECS CLI\. To obtain the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-push-syntax"></a>

**ecs\-cli push \[\-\-registry\-id *registry\_id*\] \[\-\-region *region*\] *ECR\_REPOSITORY*\[:*TAG*\] \[\-\-help\]**

## Options<a name="cmd-ecs-cli-push-options"></a>


| Name | Description | 
| --- | --- | 
|  `--registry-id registry_id`  |  Specifies the Amazon ECR registry ID to which to push the image\. By default, images are pushed to the current AWS account\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-push-examples"></a>

### Example 1<a name="cmd-ecs-cli-push-example1"></a>

This example pushes a local image called `ubuntu` to an Amazon ECR repository with the same name\.

```
ecs-cli push ubuntu
```

Output:

```
INFO[0000] Getting AWS account ID...
INFO[0000] Tagging image                                 repository="aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu" source-image=ubuntu tag=
INFO[0000] Image tagged
INFO[0001] Creating repository                           repository=ubuntu
INFO[0001] Repository created
INFO[0001] Pushing image                                 repository="aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu" tag=
INFO[0079] Image pushed
```