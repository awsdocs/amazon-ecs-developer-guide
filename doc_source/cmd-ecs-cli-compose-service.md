# ecs\-cli compose service<a name="cmd-ecs-cli-compose-service"></a>

## Description<a name="cmd-ecs-cli-compose-service-description"></a>

Manage Amazon ECS services with docker\-compose\-style commands on an ECS cluster\.

**Note**  
To run tasks with the Amazon ECS CLI instead of creating services, see [ecs\-cli compose](cmd-ecs-cli-compose.md)\.

The ecs\-cli compose service command works with a Docker compose file to create task definitions and manage services\. At this time, the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. By default, the command looks for a compose file in the current directory, called `docker-compose.yml`\. However, you can also specify a different file name or path to a compose file with the `--file` option\. This is especially useful for managing tasks and services from multiple compose files at a time with the Amazon ECS CLI\.

The ecs\-cli compose service command uses a project name with the task definitions and services that it creates\. When the CLI creates a task definition and service from a compose file, the task definition and service are called `project-name`\. By default, the project name is the name of the current working directory\. However, you can also specify your own project name with the `--project-name` option\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the CLI\. To manage tasks, services, and container instances that were not created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

The following parameters are supported in compose files for the Amazon ECS CLI:
+ `cap_add` \(Not valid for tasks using the Fargate launch type\)
+ `cap_drop` \(Not valid for tasks using the Fargate launch type\)
+ `command`
+ `cpu_shares`
**Note**  
If you are using the Compose version 3 format, `cpu_shares` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\.
+ `devices` \(Not valid for tasks using the Fargate launch type\)
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
+ `healthcheck` \(Compose file version 3 only\)
**Note**  
The `start_period` field is not supported using the compose file\. To specify a `start_period`, use the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\.
+ `hostname`
+ `image`
+ `labels`
+ `links` \(Not valid for tasks using the Fargate launch type\)
+ `log_driver` \(Compose file version 1 only\)
+ `log_opt` \(Compose file version 1 only\)
+ `logging` \(Compose file version 2 and 3\)
  + `driver`
  + `options`
+ `mem_limit` \(in bytes\)
**Note**  
If you are using the Compose version 3 format, `mem_limit` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\.
+ `mem_reservation` \(in bytes\)
**Note**  
If you are using the Compose version 3 format, `mem_reservation` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\.
+ `ports`
+ `privileged` \(Not valid for tasks using the Fargate launch type\)
+ `read_only`
+ `security_opt`
+ `shm_size` \(Compose file version 1 and 2 only and not valid for tasks using the Fargate launch type\)
+ `tmpfs` \(Not valid for tasks using the Fargate launch type\)
+ `ulimits`
+ `user`
+ `volumes`
+ `volumes_from` \(Compose file version 1 and 2 only\)
+ `working_dir`

**Important**  
The `build` directive is not supported at this time\.

For more information about Docker compose file syntax, see the [Compose file reference](https://docs.docker.com/compose/compose-file/#/compose-file-reference) in the Docker documentation\.

**Important**  
Some features described may only be available with the latest version of the ECS CLI\. To obtain the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-compose-service-syntax"></a>

**ecs\-cli compose \[\-\-verbose\] \[\-\-file *compose\-file*\] \[\-\-project\-name *project\-name*\] service \[*subcommand*\] \[*arguments*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-compose-service-options"></a>


| Name | Description | 
| --- | --- | 
|  `--verbose, --debug`  |  Increases the verbosity of command output to aid in diagnostics\. Required: No  | 
|  `--file, -f compose-file`  |  Specifies the Docker compose file to use\. At this time, the latest version of the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1 and 2\. If the `COMPOSE_FILE` environment variable is set when ecs\-cli compose is run, then the Docker compose file is set to the value of that environment variable\. Type: String Default: `./docker-compose.yml` Required: No  | 
|  `--project-name, -p project-name`  |  Specifies the project name to use\. If the `COMPOSE_PROJECT_NAME` environment variable is set when ecs\-cli compose is run, then the project name is set to the value of that environment variable\. Type: String Default: The current directory name\. Required: No  | 
|  `--task-role-arn role_value`  |  Specifies the short name or full Amazon Resource Name \(ARN\) of the IAM role that containers in this task can assume\. All containers in this task are granted the permissions that are specified in this role\. Type: String Required: No  | 
|  `--ecs-params`  |  Specifies the ECS parameters that are not native to Docker compose files\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\. Default: `./ecs-params.yml` Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Available Subcommands<a name="cmd-ecs-cli-compose-service-subcommands"></a>

The ecs\-cli compose service command supports the following subcommands and arguments:

create \[\-\-deployment\-max\-percent *n*\] \[\-\-deployment\-min\-healthy\-percent *n*\] \[\-\-load\-balancer\-name *value*\|\-\-target\-group\-arn *value*\] \[\-\-container\-name *value*\] \[\-\-container\-port *value*\] \[\-\-role *value*\] \[\-\-launch\-type *launch\_type*\] \[\-\-health\-check\-grace\-period *integer*\] \[\-\-create\-log\-groups\]  
Creates an ECS service from your compose file\. The service is created with a desired count of `0`, so no containers are started by this command\.  
The `--deployment-max-percent` option specifies the upper limit \(as a percentage of the service's `desiredCount`\) of the number of running tasks that can be running in a service during a deployment \(the default value is `200`\)\. The `--deployment-min-healthy-percent` option specifies the lower limit \(as a percentage of the service's desiredCount\) of the number of running tasks that must remain running and healthy in a service during a deployment \(the default value is `100`\)\. For more information, see [maximumPercent](service_definition_parameters.md#maximumPercent) and [minimumHealthyPercent](service_definition_parameters.md#minimumHealthyPercent)\.  
You can optionally run your service behind a load balancer\. The load balancer distributes traffic across the tasks that are associated with the service\. For more information, see [Service Load Balancing](service-load-balancing.md)\. After you create a service, the load balancer name or target group ARN, container name, and container port specified in the service definition are immutable\.  
You must create your load balancer resources before you can configure a service to use them\. Your load balancer resources should reside in the same VPC as your container instances and they should be configured to use the same subnets\. You must also add a security group rule to your container instance security group that allows inbound traffic from your load balancer\. For more information, see [Creating a Load Balancer](create-load-balancer.md)\. 
+ To configure your service to use an existing Elastic Load Balancing Classic Load Balancer, you must specify the load balancer name, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance is registered with the load balancer specified here\.
+ To configure your service to use an existing Elastic Load Balancing Application Load Balancer, you must specify the load balancer target group ARN, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance and port combination is registered as a target in the target group specified here\.
The `--health-check-grace-period` option specifies the period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks after a task has first started\. This is only valid if your service is configured to use a load balancer\. If your tasks take a while to start and respond to ELB health checks, you can specify a health check grace period of up to 1,800 seconds during which the ECS service scheduler ignores the Elastic Load Balancing health check status\. This grace period can prevent the Amazon ECS service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\.

start \[\-\-timeout *value*\] \[\-\-create\-log\-groups\] \[\-\-force\-deployment\]  
Starts one copy of each of the containers on the created ECS service\. This command updates the desired count of the service to `1`\.  
The `--timeout` option specifies the timeout value in minutes \(decimals supported\) to wait for the running task count to change\. If the running task count has not changed for the specified period of time, then the CLI times out and returns an error\. Setting the timeout to 0 causes the command to return without checking for success\. The default timeout value is 5 minutes\.  
The `--create-log-groups` option creates the CloudWatch log groups specified in your compose file\.  
The `--force-deployment` option, if included, forces a new deployment of the service\.

up \[\-\-deployment\-max\-percent *n*\] \[\-\-deployment\-min\-healthy\-percent *n*\] \[\-\-load\-balancer\-name *value*\|\-\-target\-group\-arn *value*\] \[\-\-container\-name *value*\] \[\-\-container\-port *value*\] \[\-\-role *value*\] \[\-\-timeout *value*\] \[\-\-launch\-type *launch\_type*\] \[\-\-health\-check\-grace\-period *integer*\] \[\-\-create\-log\-groups\] \[\-\-force\-deployment\]  
Creates an ECS service from your compose file \(if it does not already exist\) and runs one instance of that task on your cluster \(a combination of create and start\)\. This command updates the desired count of the service to `1`\.   
The `--deployment-max-percent` option specifies the upper limit \(as a percentage of the service's `desiredCount`\) of the number of running tasks that can be running in a service during a deployment \(the default value is `200`\)\. The `--deployment-min-healthy-percent` option specifies the lower limit \(as a percentage of the service's desiredCount\) of the number of running tasks that must remain running and healthy in a service during a deployment \(the default value is `100`\)\. For more information, see [maximumPercent](service_definition_parameters.md#maximumPercent) and [minimumHealthyPercent](service_definition_parameters.md#minimumHealthyPercent)\.  
The `--timeout` option specifies the timeout value in minutes \(decimals supported\) to wait for the running task count to change\. If the running task count has not changed for the specified period of time, then the CLI times out and returns an error\. Setting the timeout to 0 causes the command to return without checking for success\. The default timeout value is 5 minutes\.  
The `--health-check-grace-period` option specifies the period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks after a task has first started\. This is only valid if your service is configured to use a load balancer\. If your tasks take a while to start and respond to ELB health checks, you can specify a health check grace period of up to 1,800 seconds during which the ECS service scheduler ignores the Elastic Load Balancing health check status\. This grace period can prevent the Amazon ECS service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\.  
The `--create-log-groups` option creates the CloudWatch log groups specified in your compose file\.  
The `--force-deployment` option, if included, forces a new deployment of the service\.  
You can optionally run your service behind a load balancer\. The load balancer distributes traffic across the tasks that are associated with the service\. For more information, see [Service Load Balancing](service-load-balancing.md)\. After you create a service, the load balancer name or target group ARN, container name, and container port specified in the service definition are immutable\.  
You must create your load balancer resources before you can configure a service to use them\. Your load balancer resources should reside in the same VPC as your container instances and they should be configured to use the same subnets\. You must also add a security group rule to your container instance security group that allows inbound traffic from your load balancer\. For more information, see [Creating a Load Balancer](create-load-balancer.md)\. 
+ To configure your service to use an existing Elastic Load Balancing Classic Load Balancer, you must specify the load balancer name, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance is registered with the load balancer specified here\.
+ To configure your service to use an existing Elastic Load Balancing Application Load Balancer, you must specify the load balancer target group ARN, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance and port combination is registered as a target in the target group specified here\.

ps, list  
Lists all the containers in your cluster that belong to the service created with the compose project\.

scale \[\-\-deployment\-max\-percent *n*\] \[\-\-deployment\-min\-healthy\-percent *n*\] \[\-\-timeout *value*\] *n*  
Scales the desired count of the service to the specified count\.  
The `--deployment-max-percent` option specifies the upper limit \(as a percentage of the service's `desiredCount`\) of the number of running tasks that can be running in a service during a deployment \(the default value is `200`\)\. The `--deployment-min-healthy-percent` option specifies the lower limit \(as a percentage of the service's desiredCount\) of the number of running tasks that must remain running and healthy in a service during a deployment \(the default value is `100`\)\. For more information, see [maximumPercent](service_definition_parameters.md#maximumPercent) and [minimumHealthyPercent](service_definition_parameters.md#minimumHealthyPercent)\.  
The `--timeout` option specifies the timeout value in minutes \(decimals supported\) to wait for the running task count to change\. If the running task count has not changed for the specified period of time, then the CLI times out and returns an error\. Setting the timeout to 0 causes the command to return without checking for success\. The default timeout value is 5 minutes\.

stop \[\-\-timeout *value*\]  
Stops the running tasks that belong to the service created with the compose project\. This command updates the desired count of the service to `0`\.  
The `--timeout` option specifies the timeout value in minutes \(decimals supported\) to wait for the running task count to change\. If the running task count has not changed for the specified period of time, then the CLI times out and returns an error\. Setting the timeout to 0 causes the command to return without checking for success\. The default timeout value is 5 minutes\.

rm, delete, down \[\-\-timeout *value*\]  
Updates the desired count of the service to `0` and then deletes the service\.  
The `--timeout` option specifies the timeout value in minutes \(decimals supported\) to wait for the running task count to change\. If the running task count has not changed for the specified period of time, then the CLI times out and returns an error\. Setting the timeout to 0 causes the command to return without checking for success\. The default timeout value is 5 minutes\.

help  
Shows the help text for the specified command\.

## Examples<a name="cmd-ecs-cli-compose-service-examples"></a>

### Example 1<a name="cmd-ecs-cli-compose-service-example-1"></a>

This example brings up an ECS service with the project name `hello-world` from the `hello-world.yml` compose file\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml service up
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition="ecscompose-hello-world:7"
INFO[0000] Created an ECS service                        service=ecscompose-service-hello-world taskDefinition="ecscompose-hello-world:7"
INFO[0000] Updated ECS service successfully              desiredCount=1 serviceName=ecscompose-service-hello-world
INFO[0015] (service ecscompose-service-hello-world) has started 1 tasks: (task 682dc22f-8bfa-4c28-b6f8-3a916bd8f86a).  timestamp=2017-08-18 21:16:00 +0000 UTC
INFO[0060] Service status                                desiredCount=1 runningCount=1 serviceName=ecscompose-service-hello-world
INFO[0060] ECS Service has reached a stable state        desiredCount=1 runningCount=1 serviceName=ecscompose-service-hello-world
```

### Example 2<a name="cmd-ecs-cli-compose-service-example-2"></a>

This example scales the service created by the `hello-world` project to a desired count of 2\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml service scale 2
```

Output:

```
INFO[0000] Updated ECS service successfully              desiredCount=2 serviceName=ecscompose-service-hello-world
INFO[0000] Service status                                desiredCount=2 runningCount=1 serviceName=ecscompose-service-hello-world
INFO[0030] (service ecscompose-service-hello-world) has started 1 tasks: (task 80602da8-442c-48ea-a8a9-80328c302b89).  timestamp=2017-08-18 21:17:44 +0000 UTC
INFO[0075] Service status                                desiredCount=2 runningCount=2 serviceName=ecscompose-service-hello-world
INFO[0075] ECS Service has reached a stable state        desiredCount=2 runningCount=2 serviceName=ecscompose-service-hello-world
```

### Example 3<a name="cmd-ecs-cli-compose-service-example-3"></a>

This example scales the service created by the `hello-world` project to a desired count of `0` and then deletes the service\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml service rm
```

Output:

```
INFO[0000] Updated ECS service successfully              desiredCount=0 serviceName=ecscompose-service-hello-world
INFO[0000] Service status                                desiredCount=0 runningCount=2 serviceName=ecscompose-service-hello-world
INFO[0015] Service status                                desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
INFO[0015] (service ecscompose-service-hello-world) has stopped 2 running tasks: (task 682dc22f-8bfa-4c28-b6f8-3a916bd8f86a) (task 80602da8-442c-48ea-a8a9-80328c302b89).  timestamp=2017-08-18 21:25:28 +0000 UTC
INFO[0015] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
INFO[0015] Deleted ECS service                           service=ecscompose-service-hello-world
INFO[0015] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
```

### Example 4<a name="cmd-ecs-cli-compose-service-example-4"></a>

This example creates a service from the `nginx-compose.yml` compose file and configures it to use an existing Application Load Balancer\.

```
ecs-cli compose -f nginx-compose.yml service up --target-group-arn arn:aws:elasticloadbalancing:us-east-1:aws_account_id:targetgroup/ecs-cli-alb/9856106fcc5d4be8 --container-name nginx --container-port 80 --role ecsServiceRole
```

### Example 5<a name="cmd-ecs-cli-compose-service-example-5"></a>

This example creates a service from the `nginx-compose.yml` compose file and configures it to use an existing Application Load Balancer with a health check grace period of 25 seconds\.

```
ecs-cli compose -f nginx-compose.yml service up --target-group-arn arn:aws:elasticloadbalancing:us-east-1:aws_account_id:targetgroup/ecs-cli-alb/9856106fcc5d4be8 --container-name nginx --container-port 80 --role ecsServiceRole --health-check-grace-period 25
```