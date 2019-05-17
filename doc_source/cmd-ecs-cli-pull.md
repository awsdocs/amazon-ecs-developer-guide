# ecs\-cli pull<a name="cmd-ecs-cli-pull"></a>

Pull an image from an Amazon ECR repository\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-pull-syntax"></a>

**ecs\-cli pull \[\-\-registry\-id *registry\_id*\] \[\-\-region *region*\] \[\-\-verbose\] \[\-\-use\-fips\] *ECR\_REPOSITORY*\[:*TAG*\|@*DIGEST*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-pull-options"></a>


| Name | Description | 
| --- | --- | 
|  `--registry-id registry_id`  |  Specifies the Amazon ECR registry ID from which to pull the image\. By default, images are pulled from the current AWS account\. Required: No  | 
|  `--verbose, --debug`  |  Turn on debug logging\. This provides a more verbose command output to aid in diagnosing issues\. Required: No  | 
|  `--use-fips`  |  Routes calls to Amazon ECR through FIPS endpoints\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Using FIPS Endpoints<a name="cmd-ecs-cli-pull-fips"></a>

The Amazon ECS CLI supports using FIPS endpoints for calls to Amazon ECR\. To ensure that you're accessing Amazon ECR using FIPS endpoints, use the `--use-fips` flag on the push, pull, or images command\. FIPS endpoints are currently available in us\-west\-1, us\-west\-2, us\-east\-1, us\-east\-2, and AWS GovCloud \(US\)\. For more information, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

## Examples<a name="cmd-ecs-cli-pull-examples"></a>

### Example 1<a name="cmd-ecs-cli-pull-example-1"></a>

This example pulls a local image called `amazonlinux` from an Amazon ECR repository with the same name\.

```
ecs-cli pull amazonlinux
```

Output:

```
INFO[0000] Getting AWS account ID...
INFO[0000] Pulling image                                 repository="aws_account_id.dkr.ecr.us-east-1.amazonaws.com/amazonlinux" tag=
INFO[0129] Image pulled
```