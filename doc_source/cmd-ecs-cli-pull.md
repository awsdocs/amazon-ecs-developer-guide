# ecs\-cli pull<a name="cmd-ecs-cli-pull"></a>

## Description<a name="cmd-ecs-cli-pull-description"></a>

Pull an image from an Amazon ECR repository\.

## Syntax<a name="cmd-ecs-cli-pull-syntax"></a>

**ecs\-cli pull \[\-\-registry\-id *registry\_id*\] \[\-\-region *region*\] *ECR\_REPOSITORY*\[:*TAG*|@*DIGEST*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-pull-options"></a>


| Name | Description | 
| --- | --- | 
|  `--registry-id registry_id`  |  Specifies the ECR registry ID from which to pull the image\. By default, images are pulled from the current AWS account\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-pull-examples"></a>

### Example 1<a name="cmd-ecs-cli-pull-example-1"></a>

This example pulls a local image called `amazonlinux` from an ECR repository with the same name\.

```
ecs-cli pull amazonlinux
```

Output:

```
INFO[0000] Getting AWS account ID...
INFO[0000] Pulling image                                 repository="aws_account_id.dkr.ecr.us-east-1.amazonaws.com/amazonlinux" tag=
INFO[0129] Image pulled
```