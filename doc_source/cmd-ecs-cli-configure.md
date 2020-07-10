# ecs\-cli configure<a name="cmd-ecs-cli-configure"></a>

Configures the AWS Region to use, resource creation prefixes, and the Amazon ECS cluster name to use with the Amazon ECS CLI\. Stores a single named cluster configuration in the `~/.ecs/config` file\. The first cluster configuration that is created is set as the default\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Working with Multiple Cluster Configurations<a name="ECS_CLI_multiple_cluster_configurations"></a>

The following should be noted when using multiple cluster configurations:
+ Multiple cluster configurations may be stored, but one is always the default\.
+ The first cluster configuration that is stored is set as the default\.
+ Use the `ecs-cli configure default` command to change which cluster configuration is set as the default\. For more information, see [ecs\-cli configure default](cmd-ecs-cli-configure-default.md)\.
+ A non\-default cluster configuration can be referenced in a command by using the `--cluster-config` flag\.

For more information, see [ecs\-cli configure default](cmd-ecs-cli-configure-default.md)\.

**Note**  
Ensure that you are using the latest version of the Amazon ECS CLI to use all configuration options\.

## Syntax<a name="cmd-ecs-cli-configure-syntax"></a>

ecs\-cli configure \-\-cluster *cluster\_name* \-\-region *region* \[\-\-config\-name *config\_name*\] \[\-\-cfn\-stack\-name *stack\_name*\] \[\-\-default\-launch\-type *launch\_type*\] \[\-\-help\] 

## Options<a name="cmd-ecs-cli-configure-options"></a>


| Name | Description | 
| --- | --- | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: Yes  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the Region configured using either the ecs\-cli configure or aws configure commands\. Type: String Required: Yes  | 
|  `--config-name config_name`  |  Specifies the name of this cluster configuration\. This is the name that can be referenced in commands using the `--cluster-config` flag\. If this option is omitted, then the name is set to `default`\. Type: String Required: No  | 
|  `--cfn-stack-name stack_name`  |  Specifies the stack name to add to the AWS CloudFormation stack that is created on ecs\-cli up\.  It is not recommended to use this parameter\. It is included to ensure backwards compatibility with previous versions of the ECS CLI\.  Type: String Default: `amazon-ecs-cli-setup-<cluster_name>` Required: No  | 
| `--default-launch-type launch_type` | Specifies the default launch type to use\. Valid values are FARGATE or EC2\. If not specified, no default launch type is used\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.Type: StringRequired: No | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-configure-examples"></a>

### Example<a name="cmd-ecs-cli-configure-example-1"></a>

This example configures the Amazon ECS CLI to create a cluster configuration named `ecs-cli-demo`, which uses `FARGATE` as the default launch type for cluster `ecs-cli-demo` in the `us-east-1` region\.

```
ecs-cli configure --region us-east-1 --cluster ecs-cli-demo --default-launch-type FARGATE --config-name ecs-cli-demo
```

Output:

```
INFO[0000] Saved ECS CLI cluster configuration ecs-cli-demo.
```

Contents of the `~/.ecs/config` file after running the command:

```
version: v1
default: ecs-cli-demo
clusters:
  ecs-cli-demo:
    cluster: ecs-cli-demo
    region: us-east-1
    default_launch_type: FARGATE
```