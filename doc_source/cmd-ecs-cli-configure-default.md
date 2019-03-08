# ecs\-cli configure default<a name="cmd-ecs-cli-configure-default"></a>

Sets the cluster configuration to be read from by default\.

**Note**  
Unlike the AWS CLI, the Amazon ECS CLI does not expect or require that the default configuration be named `default`\. The name of a configuration does not determine whether it is default\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-configure-default-syntax"></a>

ecs\-cli configure default \-\-config\-name *config\_name* \[\-\-help\]

## Options<a name="cmd-ecs-cli-configure-default-options"></a>


| Name | Description | 
| --- | --- | 
|  `--config-name config_name`  |  Specifies the name of the cluster configuration to use by default in subsequent commands\. Type: String Required: Yes  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-configure-default-examples"></a>

### Example<a name="cmd-ecs-cli-configure-default-example-1"></a>

This example configures the Amazon ECS CLI to set the `ecs-cli-demo` cluster configuration as the default\.

```
ecs-cli configure default --config-name ecs-cli-demo
```

There is no output if the command is successful\.