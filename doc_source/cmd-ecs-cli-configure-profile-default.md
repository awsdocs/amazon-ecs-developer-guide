# ecs\-cli configure profile default<a name="cmd-ecs-cli-configure-profile-default"></a>

Sets the Amazon ECS profile to be read from by default\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-configure-profile-default-syntax"></a>

ecs\-cli configure profile default \-\-profile\-name *profile\_name* \[\-\-help\]

## Options<a name="cmd-ecs-cli-configure-profile-default-options"></a>


| Name | Description | 
| --- | --- | 
|  `--profile-name profile_name`  |  Specifies the name of the ECS profile to be marked as default\. Type: String Required: Yes  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-configure-profile-default-examples"></a>

### Example<a name="cmd-ecs-cli-configure-profile-default-example-1"></a>

This example configures the Amazon ECS CLI to set the `default` profile as the default profile to be used\.

```
ecs-cli configure profile default --profile-name default
```

There is no output if the command is successful\.