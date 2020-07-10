# ecs\-cli compose start<a name="cmd-ecs-cli-compose-start"></a>

Starts a single Amazon ECS task from the task definition created from your Compose file\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-compose-start-syntax"></a>

ecs\-cli compose start \[\-\-region *region*\] \[\-\-cluster\-config *cluster\_config\_name*\] \[\-\-ecs\-profile *ecs\_profile*\] \[\-\-aws\-profile *aws\_profile*\] \[\-\-cluster *cluster\_name*\] \[\-\-launch\-type *launch\_type*\] \[\-\-create\-log\-groups\] \[\-\-help\] 

## Options<a name="cmd-ecs-cli-compose-start-options"></a>


| Name | Description | 
| --- | --- | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--launch-type launch_type`  |  Specifies the launch type to use\. Available options are `FARGATE` or `EC2`\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\. This overrides the default launch type stored in your cluster configuration\.  Type: StringRequired: No | 
|  `--create-log-groups`  |  Creates the CloudWatch log groups specified in your Compose files\. Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-compose-start-examples"></a>

### Run a Task<a name="cmd-ecs-cli-compose-start-example-1"></a>

This example creates a task definition from the `hello-world.yml` Compose file\. Additional ECS parameters are specified for task networking configuration for the Fargate launch type\. Then a single task is run using that task definition\.

Example Docker Compose file, named `hello-world.yml`:

```
version: '3'
services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    logging:
      driver: awslogs
      options: 
        awslogs-group: tutorial
        awslogs-region: us-east-1
        awslogs-stream-prefix: nginx
```

Example ECS parameters file, named `ecs-params.yml`:

```
version: 1
task_definition:
  task_execution_role: ecsTaskExecutionRole
  ecs_network_mode: awsvpc
  task_size:
    mem_limit: 0.5GB
    cpu_limit: 256
run_params:
  network_configuration:
    awsvpc_configuration:
      subnets:
        - subnet-abcd1234
        - subnet-dbca4321
      security_groups:
        - sg-abcd1234
      assign_public_ip: ENABLED
```

Command:

```
ecs-cli compose --file hello-world.yml --ecs-params ecs-params.yml start --launch-type FARGATE --create-log-groups
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-hello-world:5
```