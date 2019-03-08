# ecs\-cli configure migrate<a name="cmd-ecs-cli-configure-migrate"></a>

Migrates a legacy configuration file \(ECS CLI v0\.6\.6 and older\) to the new configuration file format \(ECS CLI v1\.0\.0 and later\)\. The command prints a summary of the changes to be made and then asks for confirmation to proceed\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-configure-migrate-syntax"></a>

ecs\-cli configure migrate \[\-\-force\] \[\-\-help\]

## Options<a name="cmd-ecs-cli-configure-migrate-options"></a>


| Name | Description | 
| --- | --- | 
|  `--force`  |  Omits the interactive description and confirmation step that normally occurs during the configuration file migration\. Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-configure-migrate-examples"></a>

### Example<a name="cmd-ecs-cli-configure-migrate-example-1"></a>

This example migrates the legacy Amazon ECS CLI configuration file to the new YAML format\.

```
ecs-cli configure migrate
```