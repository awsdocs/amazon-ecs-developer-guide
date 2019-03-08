# ecs\-cli<a name="cmd-ecs-cli"></a>

## Description<a name="cmd-ecs-cli-description"></a>

The Amazon ECS command line interface \(CLI\) provides high\-level commands to simplify creating, updating, and monitoring clusters and tasks from a local development environment\. The Amazon ECS CLI supports [Docker Compose](https://docs.docker.com/compose/), a popular open\-source tool for defining and running multi\-container applications\.

For a quick walkthrough of the Amazon ECS CLI, see the [Tutorial: Creating a Cluster with a Fargate Task Using the Amazon ECS CLI](ecs-cli-tutorial-fargate.md)\.

Help text is available for each individual subcommand with ecs\-cli *subcommand* \-\-help\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-syntax"></a>

**ecs\-cli \[\-\-version\] \[*subcommand*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-options"></a>


| Name | Description | 
| --- | --- | 
|  `--version, -v`  |  Prints the version information for the Amazon ECS CLI\. Required: No  | 
|  `--help, -h`  |  Show the help text for the specified command\. Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-subcommands"></a>

The ecs\-cli command supports the following subcommands:

configure  
Configures your AWS credentials, the Region to use, and the ECS cluster name to use with the Amazon ECS CLI\. For more information, see [ecs\-cli configure](cmd-ecs-cli-configure.md)\.

migrate  
Migrates a legacy configuration file \(ECS CLI v0\.6\.6 and older\) to the new configuration file format \(ECS CLI v1\.0\.0 and later\)\. The command prints a summary of the changes to be made and then asks for confirmation to proceed\. For more information, see [ecs\-cli configure migrate](cmd-ecs-cli-configure-migrate.md)\.

up  
Creates the ECS cluster \(if it does not already exist\) and the AWS resources required to set up the cluster\. For more information, see [ecs\-cli up](cmd-ecs-cli-up.md)\.

down  
Deletes the AWS CloudFormation stack that was created by ecs\-cli up and the associated resources\. For more information, see [ecs\-cli down](cmd-ecs-cli-down.md)\.

scale  
Modifies the number of container instances in an ECS cluster\. For more information, see [ecs\-cli scale](cmd-ecs-cli-scale.md)\.

logs  
Retrieves container logs from CloudWatch Logs\. Only valid for tasks that use the `awslogs` driver and has a log stream prefix specified\. For more information, see [ecs\-cli logs](cmd-ecs-cli-logs.md)\.

ps  
Lists all of the running containers in an ECS cluster\. For more information, see [ecs\-cli ps](cmd-ecs-cli-ps.md)\.

push  
Pushes an image to an Amazon ECR repository\. For more information, see [ecs\-cli push](cmd-ecs-cli-push.md)\.

pull  
Pulls an image from an ECR repository\. For more information, see [ecs\-cli pull](cmd-ecs-cli-pull.md)\.

images  
Lists all of the running containers in an ECS cluster\. For more information, see [ecs\-cli images](cmd-ecs-cli-images.md)\.

license  
Prints the `LICENSE` files for the Amazon ECS CLI and its dependencies\. For more information, see [ecs\-cli license](cmd-ecs-cli-license.md)\.

compose  
Executes docker\-compose–style commands on an ECS cluster\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md)\.

help  
Shows the help text for the specified command\.