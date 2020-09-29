# ecs\-cli compose create<a name="cmd-ecs-cli-compose-create"></a>

Creates an Amazon ECS task definition from your Compose file\.

**Important**  
We don't recommend using plaintext environment variables for sensitive information, such as credential data\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-compose-create-syntax"></a>

ecs\-cli compose create \[\-\-region *region*\] \[\-\-cluster\-config *cluster\_config\_name*\] \[\-\-ecs\-profile *ecs\_profile*\] \[\-\-aws\-profile *aws\_profile*\] \[\-\-cluster *cluster\_name*\] \[\-\-launch\-type *launch\_type*\] \[\-\-create\-log\-groups\] \[\-\-tags *key1=value1,key2=value2*\] \[\-\-help\] 

## Options<a name="cmd-ecs-cli-compose-create-options"></a>


| Name | Description | 
| --- | --- | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--launch-type launch_type`  |  Specifies the launch type to use\. Available options are `FARGATE` or `EC2`\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\. This overrides the default launch type stored in your cluster configuration\.  Type: StringRequired: No | 
|  `--create-log-groups`  |  Creates the CloudWatch log groups specified in your Compose files\. Required: No  | 
|  `--tags key1=value1,key2=value2`  |  Specifies the metadata to apply to your AWS resources\. Each tag consists of a key and an optional value\. Tags use the following format: `key1=value1,key2=value2,key3=value3`\. Amazon ECS managed tags are enabled by default if you have opted in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) formats unless you specifically disable them using the `--disable-ecs-managed-tags` flag\. For more information, see [Tagging Resources](cmd-ecs-cli-compose-service-up.md#cmd-ecs-cli-compose-service-up-tags)\. Type: Key value pairs Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Tagging Resources<a name="cmd-ecs-cli-compose-create-tags"></a>

The Amazon ECS CLI supports adding metadata in the form of resource tags to your AWS resources\. Each tag consists of a key and an optional value\. Resource tags can be used for cost allocation, automation, and access control\. For more information, see [Tagging your Amazon ECS resources](ecs-using-tags.md)\.

When using the `ecs-cli compose create` command, using the `--tags` flag enables you to add metadata tags to the task definition\.

## Examples<a name="cmd-ecs-cli-compose-create-examples"></a>

### Register a Task Definition<a name="cmd-ecs-cli-compose-create-example-1"></a>

This example creates a task definition with the project name `hello-world` from the `hello-world.yml` Compose file\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml create --launch-type EC2
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-hello-world:5
```

### Register a Task Definition Using the EC2 Launch Type Without Task Networking<a name="cmd-ecs-cli-compose-create-example-2"></a>

This example creates a task definition with the project name `hello-world` from the `hello-world.yml` Compose file\. Additional ECS parameters specified for the container size parameters\.

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
        awslogs-group: /ecs/cli/tutorial
        awslogs-region: us-east-1
        awslogs-stream-prefix: nginx
```

Example ECS parameters file, named `ecs-params.yml`:

```
version: 1
task_definition:
  services:
    nginx:
      cpu_shares: 256
      mem_limit: 0.5GB
      mem_reservation: 0.5GB
```

```
ecs-cli compose --project-name hello-world --file hello-world.yml --ecs-params ecs-params.yml --region us-east-1 create --launch-type EC2
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-hello-world:5
```

### Register a Task Definition Using the Fargate Launch Type<a name="cmd-ecs-cli-compose-create-example-3"></a>

This example creates a task definition with the project name `hello-world` from the `hello-world.yml` Compose file\. Additional ECS parameters are specified for task networking configuration for the Fargate launch type\. Then one instance of the task is run\.

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
ecs-cli compose --project-name hello-world --file hello-world.yml --ecs-params ecs-params.yml --region us-east-1 create --launch-type FARGATE
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-hello-world:5
```