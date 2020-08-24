# ecs\-cli local<a name="cmd-ecs-cli-local"></a>

Runs your Amazon ECS tasks locally by creating a Docker Compose file from an Amazon ECS task definition\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-local-syntax"></a>

**ecs\-cli local \[*subcommand*\] \[*arguments*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-local-options"></a>


| Name | Description | 
| --- | --- | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the Region configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-local-subcommands"></a>

The ecs\-cli local command supports the following subcommands\. Each of these subcommands has their own flags associated with them, which can be displayed using the `--help` flag\.

create  
Creates a Docker Compose file from an Amazon ECS task definition\. For more information, see [ecs\-cli local create](cmd-ecs-cli-local-create.md)\.

up  
Runs containers locally from an Amazon ECS task definition\. For more information, see [ecs\-cli local up](cmd-ecs-cli-local-up.md)\.

down  
Stops and removes locally running containers\. For more information, see [ecs\-cli local down](cmd-ecs-cli-local-down.md)\.

ps  
Lists locally running containers\. For more information, see [ecs\-cli local ps](cmd-ecs-cli-local-ps.md)\.

help  
Shows the help text for the specified command\.