# ecs\-cli configure profile default<a name="cmd-ecs-cli-configure-profile-default"></a>

## Description<a name="cmd-ecs-cli-configure-profile-default-description"></a>

Sets the Amazon ECS profile to be read from by default\.

## Syntax<a name="cmd-ecs-cli-configure-profile-default-syntax"></a>

**ecs\-cli configure profile default \-\-profile\-name *profile\_name***

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