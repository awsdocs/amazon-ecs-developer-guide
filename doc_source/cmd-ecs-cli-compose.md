# ecs\-cli compose<a name="cmd-ecs-cli-compose"></a>

## Description<a name="cmd-ecs-cli-compose-description"></a>

Manage Amazon ECS tasks with docker\-compose\-style commands on an ECS cluster\.

**Note**  
To create Amazon ECS services with the Amazon ECS CLI, see [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.

The ecs\-cli compose command works with a Docker compose file to create task definitions and manage tasks\. At this time, the latest version of the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1 and 2\. By default, the command looks for a compose file in the current directory, called `docker-compose.yml`\. However, you can also specify a different file name or path to a compose file with the `--file` option\. This is especially useful for managing tasks and services from multiple compose files at a time with the Amazon ECS CLI\.

The ecs\-cli compose command uses a project name with the task definitions and services it creates\. When the CLI creates a task definition from a compose file, the task definition is called `ecscompose-project-name`\. When the CLI creates a service from a compose file, the service is called `ecscompose-service-project-name`\. By default, the project name is the name of the current working directory\. However, you can also specify your own project name with the `--project-name` option\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the CLI\. To manage tasks, services, and container instances that were not created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

The following parameters are supported in compose files for the Amazon ECS CLI: 

+ `cap_add` \(Not valid for tasks using the Fargate launch type\)

+ `cap_drop` \(Not valid for tasks using the Fargate launch type\)

+ `command`

+ `cpu_shares`

+ `dns`

+ `dns_search`

+ `entrypoint`

+ `environment`: If an environment variable value is not specified in the compose file, but it exists in the shell environment, the shell environment variable value is passed to the task definition that is created for any associated tasks or services\.
**Important**  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.

+ `env_file`
**Important**  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.

+ `extra_hosts`

+ `hostname`

+ `image`

+ `labels`

+ `links` \(Not valid for tasks using the Fargate launch type\)

+ `log_driver` \(Compose file version 1 only\)

+ `log_opt` \(Compose file version 1 only\)

+ `logging` \(Compose file version 2 only\)

  + `driver`

  + `options`

+ `mem_limit` \(in bytes\)

+ `mem_reservation` \(in bytes\)

+ `ports`

+ `privileged` \(Not valid for tasks using the Fargate launch type\)

+ `read_only`

+ `security_opt`

+ `ulimits`

+ `user`

+ `volumes`

+ `volumes_from`

+ `working_dir`

**Important**  
The `build` directive is not supported at this time\.

For more information about Docker compose file syntax, see the [Compose file reference](https://docs.docker.com/compose/compose-file/#/compose-file-reference) in the Docker documentation\. 

**Note**  
Ensure you are using the latest version of the Amazon ECS CLI to use all configuration options\.

## Using Amazon ECS Parameters<a name="cmd-ecs-cli-compose-ecsparams"></a>

Since there are certain fields in an Amazon ECS task definition that do not correspond to fields in a Docker compose file, you can specify those values using the `--ecs-params` flag\. By default, the command looks for an ECS params file in the current directory, called `ecs-params.yml`\. Currently, the file supports the follow schema:

```
version: 1
task_definition:
  ecs_network_mode: string
  task_role_arn: string
  task_execution_role: string
  task_size:
    cpu_limit: string
    mem_limit: string
  services:
    <service_name>:
      essential: boolean
run_params:
  network_configuration:
    awsvpc_configuration:
      subnets: 
        - subnet_id1 
        - subnet_id2
      security_groups: 
        - secgroup_id1
        - secgroup_id2
      assign_public_ip: ENABLED
```

The fields listed under `task_definition` correspond to fields to be included in your Amazon ECS task definition\. The following are descriptions for each:

+ `ecs_network_mode` ‐ Corresponds to networkMode in an ECS task definition\. Supported values are `none`, `bridge`, `host`, or `awsvpc`\. If not specified, this defaults to `bridge`\. If you are using task networking, this field must be set to `awsvpc`\. For more information, see [Network Mode](task_definition_parameters.md#network_mode)\.

+ `task_role_arn` ‐ the name or full ARN of an IAM role to be associated with the task\. For more information, see [Task Role](task_definition_parameters.md#task_role_arn)\.

+ `task_execution_role` ‐ the name or full ARN of the task execution role\. This is a required field if you want your tasks to be able to store container application logs in CloudWatch or allow your tasks to pull container images from Amazon ECR\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

+ `task_size` ‐ the CPU and memory values for the task\. If using the EC2 launch type, this field is optional and any value can be used\. If using the Fargate launch type, this field is required and you must use one of the following sets of values for the `cpu` and `memory` parameters\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/cmd-ecs-cli-compose.html)

  For more information, see [Task Size](task_definition_parameters.md#task_size)\.

+ `services` ‐ corresponds to the services listed in your Docker compose file, with `service_name` matching the name of the container to run\. Its fields are merged into a container definition\. The only field you can specify on it is `essential`\. If not specified, the value for `essential` defaults to `true`\.

The fields listed under `run_params` are for values needed as options to API calls not specifically related to a task definition, such as `compose up` \(RunTask\) and `compose service up` \(CreateService\)\. Currently, the only supported parameter under `run_params` is `network_configuration`, which is a required parameter to use task networking\. It is required when using tasks with the Fargate launch type\.

+ `network_configuration` ‐ required field if you specified `awsvpc` for `ecs_network_mode`\. It uses one nested parameter, `awsvpc_configuration`, which has the following subfields:

  + `subnets` ‐ list of subnet IDs used to associate with your tasks\. The listed subnets must be in the same VPC and Availability Zone as the instances on which to launch your tasks\.

  + `security_groups` ‐ list of security group IDs to associate with your tasks\. The listed security must be in the same VPC as the instances on which to launch your tasks\.

  + `assign_public_ip` ‐ supported values for this field are `ENABLED` or `DISABLED`\. This field is only used for tasks using the Fargate launch type\. If this field is present in tasks using task networking with the EC2 launch type, the request fails\.

## Syntax<a name="cmd-ecs-cli-compose-syntax"></a>

**ecs\-cli compose \[\-\-verbose\] \[\-\-file *compose\-file*\] \[\-\-project\-name *project\-name*\] \[\-\-task\-role\-arn *role\_value*\] \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-ecs\-params *ecs\-params\.yml*\] \[*subcommand*\] \[*arguments*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-compose-options"></a>


| Name | Description | 
| --- | --- | 
|  `--verbose, --debug`  |  Increases the verbosity of command output to aid in diagnostics\. Required: No  | 
|  `--file, -f compose-file`  |  Specifies the Docker compose file to use\. At this time, the latest version of the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1 and 2\. If the `COMPOSE_FILE` environment variable is set when ecs\-cli compose is run, then the Docker compose file is set to the value of that environment variable\. Type: String Default: `./docker-compose.yml` Required: No  | 
|  `--project-name, -p project-name`  |  Specifies the project name to use\. If the `COMPOSE_PROJECT_NAME` environment variable is set when ecs\-cli compose is run, then the project name is set to the value of that environment variable\. Type: String Default: The current directory name\. Required: No  | 
|  `--task-role-arn role_value`  |  Specifies the short name or full Amazon Resource Name \(ARN\) of the IAM role that containers in this task can assume\. All containers in this task are granted the permissions that are specified in this role\. Type: String Required: No  | 
|  `--ecs-params`  |  Specifies the ECS parameters that are not native to Docker compose files\. For more information, see [Using Amazon ECS Parameters](#cmd-ecs-cli-compose-ecsparams)\. Default: `./ecs-params.yml` Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Type: Boolean Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-compose-subcommands"></a>

The ecs\-cli compose command supports the following subcommands\. Each of these subcommands have their own flags associated with them, which can be displayed using the `--help` flag\.

create  
Creates an Amazon ECS task definition from your compose file\. For more information, see [ecs\-cli compose create](cmd-ecs-cli-compose-create.md)\.

ps, list  
Lists all the containers in your cluster that were started by the compose project\.

run \[*containerName*\] \["*command* \.\.\."\] \.\.\.  
Starts all containers overriding commands with the supplied one\-off commands for the containers\.

scale *n*  
Scales the number of running tasks to the specified count\.

start  
Starts a single task from the task definition created from your compose file\. For more information, see [ecs\-cli compose start](cmd-ecs-cli-compose-start.md)\.

stop, down  
Stops all the running tasks created by the compose project\.

up  
Creates an ECS task definition from your compose file \(if it does not already exist\) and runs one instance of that task on your cluster \(a combination of create and start\)\. For more information, see [ecs\-cli compose up](cmd-ecs-cli-compose-up.md)\.

service \[*subcommand*\]  
Creates an ECS service from your compose file\. For more information, see [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.

help  
Shows the help text for the specified command\.