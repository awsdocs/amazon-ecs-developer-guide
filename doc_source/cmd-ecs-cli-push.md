# ecs\-cli push<a name="cmd-ecs-cli-push"></a>

Pushes an image to an Amazon ECR repository\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-push-syntax"></a>

**ecs\-cli push \[\-\-registry\-id *registry\_id*\] \[\-\-tags *key1=value1,key2=value2*\] \[\-\-region *region*\] \[\-\-verbose\] \[\-\-use\-fips\] *ECR\_REPOSITORY*\[:*TAG*\] \[\-\-help\]**

## Options<a name="cmd-ecs-cli-push-options"></a>


| Name | Description | 
| --- | --- | 
|  `--registry-id registry_id`  |  Specifies the Amazon ECR registry ID to which to push the image\. By default, images are pushed to the current AWS account\. Type: String Required: No  | 
|  `--tags value`  |  Specifies the metadata to apply to your Amazon ECR repository\. Each tag consists of a key and an optional value\. Tag keys can have a maximum character length of 128 characters, and tag values can have a maximum length of 256 characters\. Tags use the following format: `key1=value1,key2=value2,key3=value3`\. Type: Key value pairs Required: No  | 
|  `--verbose, --debug`  |  Turn on debug logging\. This provides a more verbose command output to aid in diagnosing issues\. Required: No  | 
|  `--use-fips`  |  Routes calls to Amazon ECR through FIPS endpoints\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Using FIPS Endpoints<a name="cmd-ecs-cli-push-fips"></a>

The Amazon ECS CLI supports using FIPS endpoints for calls to Amazon ECR\. To ensure that you're accessing Amazon ECR using FIPS endpoints, use the `--use-fips` flag on the push, pull, or images command\. FIPS endpoints are currently available in us\-west\-1, us\-west\-2, us\-east\-1, us\-east\-2, and AWS GovCloud \(US\)\. For more information, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

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