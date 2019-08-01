# ecs\-cli compose service<a name="cmd-ecs-cli-compose-service"></a>

Manage Amazon ECS services with docker\-compose\-style commands on an ECS cluster\. For more information on how Docker compose file syntax works with the ECS CLI, see [Using Docker Compose File Syntax](cmd-ecs-cli-compose-parameters.md)\.

**Note**  
To run tasks with the Amazon ECS CLI instead of creating services, see [ecs\-cli compose](cmd-ecs-cli-compose.md)\.

The ecs\-cli compose service command uses a project name with the task definitions and services that it creates\. When the Amazon ECS CLI creates a task definition and service from a compose file, the task definition and service are called `project-name`\. By default, the project name is the name of the directory that contains your Docker compose file\. However, you can also specify your own project name with the `--project-name` option\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the Amazon ECS CLI\. To manage tasks, services, and container instances that weren't created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-compose-service-syntax"></a>

**ecs\-cli compose \[\-\-verbose\] \[\-\-file *compose\_file*\] \[\-\-project\-name *project\_name*\] \[\-\-task\-role\-arn *task\_role\_arn*\] \[\-\-ecs\-params *ecs\_params\_file*\] \[\-\-registry\-creds *value*\] \[\-\-region *region*\] \[\-\-cluster\-config *cluster\_config\_name*\] \[\-\-ecs\-profile *ecs\_profile*\] \[\-\-aws\-profile *aws\_profile*\] \[\-\-cluster *cluster\_name*\] \[\-\-help\] service \[*subcommand*\] \[*arguments*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-compose-service-options"></a>


| Name | Description | 
| --- | --- | 
|  `--verbose, --debug`  |  Increases the verbosity of command output to aid in diagnostics\. Required: No  | 
|  `--file, -f compose_file`  |  Specifies the Docker Compose file to use\. At this time, the latest version of the Amazon ECS CLI only supports the major versions of [Docker Compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. The version specified in the compose file must be the string `"1"`, `"1.0"`, `"2"`, `"2.0"`, `"3"`, or `"3.0"`\. Docker Compose minor versions are not supported\. If the `COMPOSE_FILE` environment variable is set when ecs\-cli compose is run, the Docker Compose file is set to the value of that environment variable\. Type: String Default: `./docker-compose.yml` Required: No  | 
|  `--project-name, -p project_name`  |  Specifies the project name to use\. If the `COMPOSE_PROJECT_NAME` environment variable is set when ecs\-cli compose is run, the project name is set to the value of that environment variable\. Type: String Default: The current directory name\. Required: No  | 
|  `--task-role-arn role_value`  |  Specifies the short name or full Amazon Resource Name \(ARN\) of the IAM role that containers in this task can assume\. All containers in this task are granted the permissions that are specified in this role\. Type: String Required: No  | 
|  `--ecs-params ecs_params_file`  |  Specifies the ECS parameters that aren't native to Docker Compose files\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\. Default: `./ecs-params.yml` Required: No  | 
|  `--registry-creds value`  |  Specifies the Amazon ECS registry credentials file to use\. Defaults to the latest output file from the `ecs-cli registry-creds up` command, if one exists\. For more information, see [ecs\-cli registry\-creds](cmd-ecs-cli-registry-creds.md)\. Default: `./ecs-registry-creds_[TIMESTAMP].yml` Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-compose-service-subcommands"></a>

The ecs\-cli compose service command supports the following subcommands\. Each of these subcommands has their own flags associated with them, which can be displayed using the `--help` flag\.

create  
Creates an Amazon ECS service from your compose file\. The service is created with a desired count of `0`, so no containers are started by this command\. For more information, see [ecs\-cli compose service create](cmd-ecs-cli-compose-service-create.md)\.

start  
Starts one copy of each of the containers on the created Amazon ECS service\. This command updates the desired count of the service to `1`\. For more information, see [ecs\-cli compose service start](cmd-ecs-cli-compose-service-start.md)\.

up  
Creates an Amazon ECS service from your compose file \(if it does not already exist\) and runs one instance of that task on your cluster \(a combination of create and start\)\. This command updates the desired count of the service to `1`\. For more information, see [ecs\-cli compose service up](cmd-ecs-cli-compose-service-up.md)\.

ps, list  
Lists all the containers in your cluster that belong to the service created with the compose project\. For more information, see [ecs\-cli compose service ps, list](cmd-ecs-cli-compose-service-ps.md)\.

scale  
Scales the desired count of the service to the specified count\. For more information, see [ecs\-cli compose service scale](cmd-ecs-cli-compose-service-scale.md)\.

stop  
Stops the running tasks that belong to the service created with the compose project\. This command updates the desired count of the service to `0`\. For more information, see [ecs\-cli compose service stop](cmd-ecs-cli-compose-service-stop.md)\.

rm, delete, down  
Updates the desired count of the service to `0` and then deletes the service\. For more information, see [ecs\-cli compose service rm, delete, down](cmd-ecs-cli-compose-service-rm.md)\.