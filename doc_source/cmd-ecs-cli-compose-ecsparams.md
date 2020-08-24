# Using Amazon ECS Parameters<a name="cmd-ecs-cli-compose-ecsparams"></a>

When using the `ecs-cli compose` or `ecs-cli compose service` commands to manage your Amazon ECS tasks and services, there are certain fields in an Amazon ECS task definition that do not correspond to fields in a Docker compose file\. You can specify those values using an ECS parameters file with the `--ecs-params` flag\. By default, the command looks for an ECS parameters file in the current directory named `ecs-params.yml`\. However, you can also specify a different file name or path to an ECS parameters file with the `--ecs-params` option\.

Currently, the file supports the follow schema:

```
version: 1
task_definition:
  ecs_network_mode: string
  task_role_arn: string
  task_execution_role: string
  task_size:
    cpu_limit: string
    mem_limit: string
  pid_mode: string
  ipc_mode: string
  services:
    <service_name>:
      essential: boolean
      repository_credentials:
        credentials_parameter: string
      cpu_shares: integer
      mem_limit: string
      mem_reservation: string
      gpu: string
      init_process_enabled: boolean
      healthcheck:
        test: ["CMD", "curl -f http://localhost"]
        interval: string
        timeout: string
        retries: integer
        start_period: string
      firelens_configuration:
        type: string
        options:
          enable-ecs-log-metadata: boolean
      secrets:
        - value_from: string
          name: string
  docker_volumes:
      - name: string
        scope: string
        autoprovision: 
        driver: string
        driver_opts: boolean
           string: string
        labels:
           string: string
  efs_volumes:
      - name: string
        filesystem_id: string
        root_directory: string
        transit_encryption: string
        transit_encryption_port: int64
        access_point: string
        iam: string
  placement_constraints:
      - type: string
        expression: string
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
  task_placement:
    strategy:
      - type: string
        field: string
    constraints:
      - type: string
        expression: string
  service_discovery:
    container_name: string
    container_port: integer
    private_dns_namespace:
        vpc: string
        id: string
        name: string
        description: string
    public_dns_namespace:
        id: string
        name: string
    service_discovery_service:
        name: string
        description: string
        dns_config:
          type: string
          ttl: integer
        healthcheck_custom_config:
          failure_threshold: integer
```

The fields listed under `task_definition` correspond to fields to be included in your Amazon ECS task definition\.
+ `ecs_network_mode` – Corresponds to `networkMode` in an ECS task definition\. Supported values are `none`, `bridge`, `host`, or `awsvpc`\. The default value is `bridge`\. If you are using task networking, this field must be set to `awsvpc`\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.
+ `task_role_arn` – The name or full ARN of an IAM role to be associated with the task\. For more information, see [Task role](task_definition_parameters.md#task_role_arn)\.
+ `task_execution_role` – The name or full ARN of the task execution role\. This is a required field if you want your tasks to be able to store container application logs in CloudWatch or allow your tasks to pull container images from Amazon ECR\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.
+ `task_size` – The CPU and memory values for the task\. If you are using the EC2 launch type, this field is optional and any value can be used\. If using the Fargate launch type, this field is required and you must use one of the following sets of values for the `cpu` and `memory` parameters\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/cmd-ecs-cli-compose-ecsparams.html)

  For more information, see [Task size](task_definition_parameters.md#task_size)\.
+ `pid_mode` – The process namespace to use for the containers in the task\. The valid values are `host` or `task`\. If `host` is specified, then all containers within the tasks that specified the `host` PID mode on the same container instance share the same IPC resources with the host Amazon EC2 instance\. If `task` is specified, all containers within the specified task share the same process namespace\. If no value is specified, the default is a private namespace\. For more information, see [PID settings](https://docs.docker.com/engine/reference/run/#pid-settings) in the *Docker run reference*\.

  If the `host` PID mode is used, be aware that there is a heightened risk of undesired process namespace expose\. For more information, see [Docker security](https://docs.docker.com/engine/security/security/)\.
**Note**  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.
+ `ipc_mode` – The IPC resource namespace to use for the containers in the task\. The valid values are `host`, `task`, or `none`\. If `host` is specified, then all containers within the tasks that specified the `host` IPC mode on the same container instance share the same IPC resources with the host Amazon EC2 instance\. If `task` is specified, all containers within the specified task share the same IPC resources\. If `none` is specified, then IPC resources within the containers of a task are private and not shared with other containers in a task or on the container instance\. If no value is specified, then the IPC resource namespace sharing depends on the Docker daemon setting on the container instance\. For more information, see [IPC settings](https://docs.docker.com/engine/reference/run/#ipc-settings) in the *Docker run reference*\.

  If the `host` IPC mode is used, be aware that there is a heightened risk of undesired IPC namespace expose\. For more information, see [Docker security](https://docs.docker.com/engine/security/security/)\.

  If you are setting namespaced kernel parameters using `systemControls` for the containers in the task, the following will apply to your IPC resource namespace\. For more information, see [System Controls](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html) in the *Amazon Elastic Container Service Developer Guide*\.
  + For tasks that use the `host` IPC mode, IPC namespace related `systemControls` are not supported\.
  + For tasks that use the `task` IPC mode, IPC namespace related `systemControls` will apply to all containers within a task\.
**Note**  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.
+ `services` – Corresponds to the services listed in your Docker compose file, with `service_name` matching the name of the container to run\. Its fields are merged into a container definition\.
  + `essential` – If the `essential` parameter of a container is marked as `true`, and that container fails or stops for any reason, all other containers that are part of the task are stopped\. If the `essential` parameter of a container is marked as `false`, then its failure does not affect the rest of the containers in a task\. The default value is `true`\.

    All tasks must have at least one essential container\. If you have an application that is composed of multiple containers, you should group containers that are used for a common purpose into components, and separate the different components into multiple task definitions\.
  + `repository_credentials` – If you are using a private repository for pulling images, `repository_credentials` allows you to specify an AWS Secrets Manager secret ARN for the name of the secret containing your private repository credentials as a `credential_parameter`\. For more information, see [Private registry authentication for tasks](private-auth.md)\.
  + `cpu_shares` – This parameter maps to `cpu_shares` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `mem_limit` – This parameter maps to `mem_limit` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `mem_reservation` – This parameter maps to `mem_reservation` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `gpu` – The number of physical GPUs the Amazon ECS container agent will reserve for the container\. This parameter maps to the `resourceRequirements` field in a task definition\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.
  + `init_process_enabled` – This parameter enables you to run an `init` process inside the container that forwards signals and reaps processes\. This parameter maps to the `--init` option to [docker run](https://docs.docker.com/engine/reference/run/)\.

    This parameter requires version 1\.25 of the Docker Remote API or greater on your container instance\.
  + `healthcheck` – This parameter maps to `healthcheck` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/#healthcheck)\. The `test` field can also be specified as `command` and must be either a string or a list\. If it's a list, the first item must be either `NONE`, `CMD`, or `CMD-SHELL`\. If it's a string, it's equivalent to specifying `CMD-SHELL` followed by that string\. The `interval`, `timeout`, and `start_period` fields are specified as durations in a string format\. For example: `2.5s`, `10s`, `1m30s`, `2h23m`, or `5h34m56s`\.
**Note**  
If no units are specified, seconds are assumed\. For example, you can specify either `10s` or simply `10`\.
  + `firelens_configuration` – This parameter allows you to define a log configuration using the `awsfirelens` log driver\. This is used to route logs to an AWS service or partner destination for log storage and analytics\. For more information, see [Custom log routing](using_firelens.md)\.
    + `type` – The log router type to use\. Supported options are `fluentbit` and `fluentd`\.
    + `options` – The log router options to use\. This will depend on the destination you are routing your logs to\. For more information, see [Custom log routing](using_firelens.md)\.
  + `secrets` – This parameter allows you to inject sensitive data into your containers by storing your sensitive data in AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.
    + `value_from` – This is the AWS Systems Manager Parameter Store ARN or name to expose to the container\. If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.
    + `name` – The value to set as the environment variable on the container\.
+ `docker_volumes` – This parameter allows you to create docker volumes\. The `name` key is required, and `scope`, `autoprovision`, `driver`, `driver_opts` and `labels` correspond with the Docker volume configuration fields in a task definition\. For more information, see [DockerVolumeConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DockerVolumeConfiguration.html) in the *Amazon Elastic Container Service API Reference*\. Volumes defined with the `docker_volumes` key can be referenced in your compose file by name, even if they were not also specified in the compose file\.
+ `efs_volumes` – This parameter enables you to mount Amazon EFS file systems\. The `name` and `filesystem_id` keys are required\.
  + `name` – The name of the volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. This name is referenced in the sourceVolume parameter of container definition mountPoints\.
  + `filesystem_id` – The ID of the file system for which to create the mount target\.
  + `root_directory` – The directory within the Amazon EFS file system to mount as the root directory inside the host\. If this parameter is omitted, the root of the Amazon EFS volume will be used\. Specifying / will have the same effect as omitting this parameter\.
  + `transit_encryption` – Whether or not to enable encryption for Amazon EFS data in transit between the Amazon ECS host and the Amazon EFS server\. This parameter is required if IAM is enabled or an access point ID is specified\. Valid values are `ENABLED or DISABLED. DISABLED` is the default\.
  + `transit_encryption_port` – The port to use when sending encrypted data between the Amazon ECS host and the Amazon EFS server\. If you do not specify a transit encryption port, it will use the port selection strategy that the Amazon EFS mount helper uses\. This parameter is required if `transit_encryption` is enabled\.
  + `access_point` – The ID of the Amazon EFS access point to use\. If an access point is specified, the root directory value will be relative to the directory set for the access point\. If specified, `transit_encryption` must be enabled\.
  + `iam` – Whether or not to use the Amazon ECS task IAM role defined in a task definition when mounting the Amazon EFS file system\. If enabled, `transit encryption` must be enabled in the EFSVolumeConfiguration\. Valid values are `ENABLED or DISABLED. DISABLED` is the default\.
+ `placement_constraints` – This parameter allows you to specify a list of constraints on task placement within the task definition\. For more information, see [TaskDefinitionPlacementConstraint](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TaskDefinitionPlacementConstraint.html) in the *Amazon Elastic Container Service API Reference*\. It is optional if you are using the EC2 launch type\. It is not supported if using the Fargate launch type\.

The fields listed under `run_params` are for values needed as options to any API calls not specifically related to a task definition, such as `compose up` \(RunTask\) and `compose service up` \(CreateService\)\.
+ `network_configuration` – Required if you specified `awsvpc` for `ecs_network_mode`\. It uses one nested parameter, `awsvpc_configuration`, which has the following subfields:
  + `subnets` – A list of subnet IDs used to associate with your tasks\. The listed subnets must be in the same VPC and Availability Zone as the instances on which to launch your tasks\.
  + `security_groups` – A list of security group IDs to associate with your tasks\. The listed security must be in the same VPC as the instances on which to launch your tasks\.
  + `assign_public_ip` – The supported values for this field are `ENABLED` or `DISABLED`\. This field is only used for tasks using the Fargate launch type\. If this field is present in tasks using task networking with the EC2 launch type, the request fails\.
+ `task_placement` – This parameter allows you to specify task placement options\. It is optional if you are using the EC2 launch type\. It is not supported if using the Fargate launch type\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  It has the following subfields:
  + `strategy` – A list of objects, with two keys\. Valid keys are `type` and `field`\.
    + `type` – Valid values are `random`, `binpack`, or `spread`\. If `random` is specified, the `field` key should not be provided\.
    + `field` – Valid values depend on the strategy type\.
      + For `spread`, valid values are `instanceId`, `host`, or attribute key\-value pairs, for example `attribute:ecs.instance-type =~ t2.*`\.
      + For `binpack`, valid values are `cpu` or `memory`\.
  + `constraints` – A list of objects, with two keys\. Valid keys are `type` and `expression`\.
    + `type` – Valid values are `distinctInstance` and `memberOf`\. If `distinctInstance` is specified, the `expression` key should not be provided\.
    + `expression` – When type is `memberOf`, valid values are key\-value pairs for attributes or task groups, for example `task:group == databases` or `attribute:color =~ green`\.
+ `service_discovery` – This parameter allows you to configure Amazon ECS Service Discovery using Amazon Route 53 auto naming API actions to manage DNS entries for your service's tasks\. For more information, see [Tutorial: Creating an Amazon ECS Service That Uses Service Discovery Using the Amazon ECS CLI](ecs-cli-tutorial-servicediscovery.md)\.