# Task definition parameters<a name="task_definition_parameters"></a>

Task definitions are split into separate parts: the task family, the IAM task role, the network mode, container definitions, volumes, task placement constraints, and launch types\. The family and container definitions are required in a task definition\. In contrast, task role, network mode, volumes, task placement constraints, and launch type are optional\.

You can use these parameters in a JSON file to configure your task definition\. For more information, see [Example task definitions](firelens-example-taskdefs.md)\.

The following are more detailed descriptions for each task definition parameter\.

## Family<a name="family"></a>

`family`  
Type: string  
Required: yes  
When you register a task definition, you give it a family, which is similar to a name for multiple versions of the task definition, specified with a revision number\. The first task definition that's registered into a particular family is given a revision of 1, and any task definitions registered after that are given a sequential revision number\.

## Launch types<a name="requires_compatibilities"></a>

When you register a task definition, you can specify a launch type that Amazon ECS should validate the task definition against\. If the task definition doesn't validate against the compatibilities specified, a client exception is returned\. For more information, see [Amazon ECS launch types](launch_types.md)\.

The following parameter is allowed in a task definition\.

`requiresCompatibilities`  
Type: string array  
Required: no  
Valid Values: `EC2` \| `FARGATE` \| `EXTERNAL`  
The launch type to validate the task definition against\. This initiates a check to ensure that all of the parameters that are used in the task definition meet the requirements of the launch type\.

## Task role<a name="task_role_arn"></a>

`taskRoleArn`  
Type: string  
Required: no  
When you register a task definition, you can provide a task role for an IAM role that allows the containers in the task permission to call the AWS APIs that are specified in its associated policies on your behalf\. For more information, see [Task IAM role](task-iam-roles.md)\.  
When you launch the Amazon ECS\-optimized Windows Server AMI, IAM roles for tasks on Windows require that the `-EnableTaskIAMRole` option is set\. Your containers must also run some configuration code to use the feature\. For more information, see [Additional configuration for Windows IAM roles for tasks](windows_task_IAM_roles.md)\.

## Task execution role<a name="execution_role_arn"></a>

`executionRoleArn`  
Type: string  
Required: conditional  
The Amazon Resource Name \(ARN\) of the task execution role that grants the Amazon ECS container agent permission to make AWS API calls on your behalf\.   
The task execution IAM role is required depending on the requirements of your task\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

## Network mode<a name="network_mode"></a>

`networkMode`  
Type: string  
Required: no  
The Docker networking mode to use for the containers in the task\. For Amazon ECS tasks that are hosted on Amazon EC2 Linux instances, the valid values are `none`, `bridge`, `awsvpc`, and `host`\. If no network mode is specified, the default network mode is `bridge`\. For Amazon ECS tasks hosted on Amazon EC2 Windows instances, the valid values are `default`, and `awsvpc`\. If no network mode is specified, the `default` network mode is used\.   
If the network mode is set to `none`, the task's containers don't have external connectivity and port mappings can't be specified in the container definition\.  
If the network mode is `bridge`, the task uses Docker's built\-in virtual network on Linux, which runs inside each Amazon EC2 instance that hosts the task\. The built\-in virtual network on Linux uses the `bridge` Docker network driver\.\.  
If the network mode is `host`, the task uses the host's network which bypasses Docker's built\-in virtual network by mapping container ports directly to the ENI of the Amazon EC2 instance that hosts the task\. Dynamic port mappings can’t be used in this network mode\. A container in a task definition that uses this mode must specify a specific `hostPort` number\. A port number on a host can’t be used by multiple tasks\. As a result, you can’t run multiple tasks of the same task definition on a single Amazon EC2 instance\.  
When running tasks that use the `host` network mode, do not run containers using the root user \(UID 0\) for better security\. As a security best practice, always use a non\-root user\.
If the network mode is `awsvpc`, the task is allocated an elastic network interface, and you must specify a `NetworkConfiguration` when you create a service or run a task with the task definition\. For more information, see [Amazon ECS task networking](task-networking.md)\. Currently, only the Amazon ECS\-optimized AMI, other Amazon Linux variants with the `ecs-init` package, or AWS Fargate infrastructure support the `awsvpc` network mode\.  
If the network mode is `default`, the task uses Docker's built\-in virtual network on Windows, which runs inside each Amazon EC2 instance that hosts the task\. The built\-in virtual network on Windows uses the `nat` Docker network driver\.\.   
The `host` and `awsvpc` network modes offer the highest networking performance for containers because they use the Amazon EC2 network stack\. With the `host` and `awsvpc` network modes, exposed container ports are mapped directly to the corresponding host port \(for the `host` network mode\) or the attached elastic network interface port \(for the `awsvpc` network mode\)\. Because of this, you can't use dynamic host port mappings\.  
If using the Fargate launch type, the `awsvpc` network mode is required\. If using the EC2 launch type, the allowable network mode depends on the underlying EC2 instance's operating system\. If Linux, any network mode can be used\. If Windows, the `default`, and `awsvpc` modes can be used\. 

## Runtime platform<a name="runtime-platform"></a>

The following parameters are required for Fargate launch types\.

`operatingSystemFamily`  
Type: string  
Required: Conditional  
Default: LINUX  
This parameter is required for Amazon ECS tasks that are hosted on Fargate\.  
When you register a task definition, you specify the operating system family\.   
The valid values for Amazon ECS tasks that are hosted on Fargate are `LINUX`, `WINDOWS_SERVER_2019_FULL`, `WINDOWS_SERVER_2019_CORE`, `WINDOWS_SERVER_2022_FULL`, and `WINDOWS_SERVER_2022_CORE`\.  
The valid values for Amazon ECS tasks hosted on EC2 are `LINUX`, `WINDOWS_SERVER_2022_CORE`, `WINDOWS_SERVER_2022_FULL`, `WINDOWS_SERVER_2019_FULL`, and `WINDOWS_SERVER_2019_CORE`, `WINDOWS_SERVER_2016_FULL`, `WINDOWS_SERVER_2004_CORE`, and `WINDOWS_SERVER_20H2_CORE`\.  
All task definitions that are used in a service must have the same value for this parameter\.  
When a task definition is part of a service, this value must match the service `platformFamily` value\.

`cpuArchitecture`  
Type: string  
Required: Conditional  
Default: X86\_64  
This parameter is required for Amazon ECS tasks hosted on Fargate\.  
When you register a task definition, you specify the CPU architecture\. The valid values are `X86_64` and `ARM64`\.  
All task definitions that are used in a service must have the same value for this parameter\.  
When you have Linux tasks for either the Fargate launch type, or the EC2 launch type, you can set the value to `ARM64`\. For more information, see [Working with 64\-bit ARM workloads on Amazon ECS](ecs-arm64.md)\.

## Task size<a name="task_size"></a>

When you register a task definition, you can specify the total CPU and memory used for the task\. This is separate from the `cpu` and `memory` values at the container definition level\. For tasks that are hosted on Amazon EC2 instances, these fields are optional\. For tasks that are hosted on Fargate \(both Linux and Windows\), these fields are required and there are specific values for both `cpu` and `memory` that are supported\.

**Note**  
Task\-level CPU and memory parameters are ignored for Windows containers\. We recommend specifying container\-level resources for Windows containers\.

The following parameter is allowed in a task definition:

`cpu`  
Type: string  
Required: conditional  
This parameter is not supported for Windows containers\.
The hard limit of CPU units to present for the task\. It can be expressed as an integer using CPU units \(for example, `1024`\) or as a string using vCPUs \(for example, `1 vCPU` or `1 vcpu`\) in a task definition\. When the task definition is registered, a vCPU value is converted to an integer indicating the CPU units\.  
For tasks that run on EC2 or external instances, this field is optional\. If your cluster doesn't have any registered container instances with the requested CPU units available, the task fails\. Supported values for tasks that run on EC2 or external instances are between `128` CPU units \(`0.125` vCPUs\) and `10240` CPU units \(`10` vCPUs\)\.  
For tasks that run on Fargate \(both Linux and Windows containers\), this field is required and you must use one of the following values, which determines your range of supported values for the `memory` parameter:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)

`memory`  
Type: string  
Required: conditional  
This parameter is not supported for Windows containers\.
The hard limit of memory \(in MiB\) to present to the task\. It can be expressed as an integer using MiB \(for example `1024`\) or as a string using GB \(for example `1GB` or `1 GB`\) in a task definition\. When the task definition is registered, a GB value is converted to an integer indicating the MiB\.  
For tasks that are hosted on Amazon EC2 instances, this field is optional and any value can be used\. If a task\-level memory value is specified, then the container\-level memory value is optional\. If your cluster doesn't have any registered container instances with the requested memory available, the task fails\. You can maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type\. For more information, see [Container Instance Memory Management](memory-management.md)\.  
For tasks hosted on Fargate \(both Linux and Windows containers\), this field is required and you must use one of the following values, which determines your range of supported values for the `cpu` parameter:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)

## Container definitions<a name="container_definitions"></a>

When you register a task definition, you must specify a list of container definitions that are passed to the Docker daemon on a container instance\. The following parameters are allowed in a container definition\.

**Topics**
+ [Standard container definition parameters](#standard_container_definition_params)
+ [Advanced container definition parameters](#advanced_container_definition_params)
+ [Other container definition parameters](#other_container_definition_params)

### Standard container definition parameters<a name="standard_container_definition_params"></a>

The following task definition parameters are either required or used in most container definitions\.

**Topics**
+ [Name](#container_definition_name)
+ [Image](#container_definition_image)
+ [Memory](#container_definition_memory)
+ [Port mappings](#container_definition_portmappings)

#### Name<a name="container_definition_name"></a>

`name`  
Type: string  
Required: yes  
The name of a container\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. If you're linking multiple containers in a task definition, the `name` of one container can be entered in the `links` of another container\. This is to connect the containers\.

#### Image<a name="container_definition_image"></a>

`image`  
Type: string  
Required: yes  
The image used to start a container\. This string is passed directly to the Docker daemon\. Images in the Docker Hub registry are available by default\. You can also specify other repositories with either `repository-url/image:tag` or `repository-url/image@digest`\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, underscores, colons, periods, forward slashes, and number signs are allowed\. This parameter maps to `Image` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `IMAGE` parameter of [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
+ When a new task starts, the Amazon ECS container agent pulls the latest version of the specified image and tag for the container to use\. However, subsequent updates to a repository image are not propagated to already running tasks\.
+ Images in private registries are supported\. For more information, see [Private registry authentication for tasks](private-auth.md)\.
+ Images in Amazon ECR repositories can be specified by using either the full `registry/repository:tag` or `registry/repository@digest` naming convention \(for example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest` or `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app@sha256:94afd1f2e64d908bc90dbca0035a5b567EXAMPLE`\)\.
+ Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\)\.
+ Images in other online repositories are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu`\)\.

#### Memory<a name="container_definition_memory"></a>

`memory`  
Type: integer  
Required: conditional  
The amount \(in MiB\) of memory to present to the container\. If your container attempts to exceed the memory specified here, the container is killed\. The total amount of memory reserved for all containers within a task must be lower than the task `memory` value, if one is specified\. This parameter maps to `Memory` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--memory` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
If using the Fargate launch type, this parameter is optional\.  
If using the EC2 launch type, you must specify either a task\-level memory value or a container\-level memory value\. If you specify both a container\-level `memory` and `memoryReservation` value, `memory` must be greater than or equal to `memoryReservation`\. If you specify `memoryReservation`, then that value is subtracted from the available memory resources for the container instance that the container is placed on\. Otherwise, the value of `memory` is used\.  
The Docker 20\.10\.0 or later daemon reserves a minimum of 6 MiB of memory for a container\. So, don't specify less than 6 MiB of memory for your containers\.  
The Docker 19\.03\.13\-ce or earlier daemon reserves a minimum of 4 MiB of memory for a container\. So, don't specify less than 4 MiB of memory for your containers\.  
If you're trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, see [Container Instance Memory Management](memory-management.md)\.

`memoryReservation`  
Type: integer  
Required: no  
The soft limit \(in MiB\) of memory to reserve for the container\. When system memory is under contention, Docker attempts to keep the container memory to this soft limit\. However, your container can consume more memory when needed, up to either the hard limit that's specified with the `memory` parameter \(if applicable\), or all of the available memory on the container instance, whichever comes first\. This parameter maps to `MemoryReservation` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--memory-reservation` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
If a task\-level memory value isn't specified, you must specify a non\-zero integer for one or both of `memory` or `memoryReservation` in a container definition\. If you specify both, `memory` must be greater than or equal to `memoryReservation`\. If you specify `memoryReservation`, then that value is subtracted from the available memory resources for the container instance on which the container is placed\. Otherwise, the value of `memory` is used\.  
For example, if your container normally uses 128 MiB of memory, but occasionally bursts to 256 MiB of memory for short periods of time, you can set a `memoryReservation` of 128 MiB, and a `memory` hard limit of 300 MiB\. This configuration allows the container to only reserve 128 MiB of memory from the remaining resources on the container instance\. At the same time, it also allows the container to use more memory resources when needed\.  
This parameter is not supported for Windows containers\.
The Docker 20\.10\.0 or later daemon reserves a minimum of 6 MiB of memory for a container\. So, don't specify less than 6 MiB of memory for your containers\.  
The Docker 19\.03\.13\-ce or earlier daemon reserves a minimum of 4 MiB of memory for a container\. So, don't specify less than 4 MiB of memory for your containers\.  
If you're trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, see [Container Instance Memory Management](memory-management.md)\.

#### Port mappings<a name="container_definition_portmappings"></a>

`portMappings`  
Type: object array  
Required: no  
Port mappings allow containers to access ports on the host container instance to send or receive traffic\.  
For task definitions that use the `awsvpc` network mode, only specify the `containerPort`\. The `hostPort` can be left blank or it must be the same value as the `containerPort`\.  
Port mappings on Windows use the `NetNAT` gateway address rather than `localhost`\. There is no loopback for port mappings on Windows, so you cannot access a container's mapped port from the host itself\.   
Most fields of this parameter \(`containerPort`, `hostPort`, `protocol`\) maps to `PortBindings` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--publish` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. If the network mode of a task definition is set to `host`, host ports must either be undefined or match the container port in the port mapping\.  
After a task reaches the `RUNNING` status, manual and automatic host and container port assignments are visible in the following locations:  
+ Console: The **Network Bindings** section of a container description for a selected task\.
+ AWS CLI: The `networkBindings` section of the describe\-tasks command output\.
+ API: The `DescribeTasks` response\.
+ Metadata: The task metadata endpoint\.  
`appProtocol`  
Type: string  
Required: no  
The application protocol that's used for the port mapping\. This parameter only applies to Service Connect\. We recommend that you set this parameter to be consistent with the protocol that your application uses\. If you set this parameter, Amazon ECS adds protocol\-specific connection handling to the service connect proxy\.If you set this parameter, Amazon ECS adds protocol\-specific telemetry in the Amazon ECS console and CloudWatch\.  
If you don't set a value for this parameter, then TCP is used\. However, Amazon ECS doesn't add protocol\-specific telemetry for TCP\.  
For more information, see [Service Connect ](service-connect.md)\.  
Valid protocol values: `"HTTP" | "HTTP2" | "GRPC" `  
`containerPort`  
Type: integer  
Required: yes, when `portMappings` are used  
The port number on the container that's bound to the user\-specified or automatically assigned host port\.  
If using containers in a task with the Fargate launch type, exposed ports must be specified using `containerPort`\.  
For Windows containers on Fargate, you can't use port 3150 for the `containerPort`\. This is because it's reserved\.  
If using containers in a task with the EC2 launch type and you specify a container port and not a host port, your container automatically receives a host port in the ephemeral port range\. For more information, see `hostPort`\. Port mappings that are automatically assigned in this way don't count toward the 100 reserved ports limit of a container instance\.  
`containerPortRange`  
Type: string  
Required: no  
The port number range on the container that's bound to the dynamically mapped host port range\.    
You can only set this parameter by using the `register-task-definition` API\. The option is available in the `portMappings` parameter\. For more information, see [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/ecs/register-task-definition.html) in the *AWS Command Line Interface Reference*\.  
The following rules apply when you specify a `containerPortRange`:  
+ You must use either the `bridge` network mode or the `awsvpc` network mode\.
+ This parameter is available for both the EC2 and AWS Fargate launch types\.
+ This parameter is available for both the Linux and Windows operating systems\.
+ The container instance must have at least version 1\.67\.0 of the container agent and at least version 1\.67\.0\-1 of the `ecs-init` package 
+ You can specify a maximum of 100 port ranges per container\.
+ You do not specify a `hostPortRange`\. The value of the `hostPortRange` is set as follows:
  + For containers in a task with the `awsvpc` network mode, the `hostPort` is set to the same value as the `containerPort`\. This is a static mapping strategy\.
  + For containers in a task with the `bridge` network mode, the Amazon ECS agent finds open host ports from the default ephemeral range and passes it to docker to bind them to the container ports\.
+ The `containerPortRange` valid values are between 1 and 65535\.
+ A port can only be included in one port mapping per container\.
+ You cannot specify overlapping port ranges\.
+ The first port in the range must be less than last port in the range\.
+ Docker recommends that you turn off the docker\-proxy in the Docker daemon config file when you have a large number of ports\.

  For more information, see [ Issue \#11185](https://github.com/moby/moby/issues/11185) on the Github website\.

  For information about how to turn off the docker\-proxy in the Docker daemon config file, see [Docker daemon](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/bootstrap_container_instance.html#bootstrap_docker_daemon) in the *Amazon ECS Developer Guide*\.
You can call [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html) to view the `hostPortRange` which are the host ports that are bound to the container ports\.  
The port ranges are not included in the Amazon ECS task events which are sent to EventBridge\. For more information, see [Amazon ECS events and EventBridge](cloudwatch_event_stream.md)\.  
`hostPortRange`  
Type: string  
Required: no  
The port number range on the host that's used with the network binding\. This is assigned is assigned by Docker and delivered by the Amazon ECS agent\.  
`hostPort`  
Type: integer  
Required: no  
The port number on the container instance to reserve for your container\.  
If using containers in a task with the Fargate launch type, the `hostPort` can either be kept blank or be the same value as `containerPort`\.  
If using containers in a task with the EC2 launch type, you can specify a non\-reserved host port for your container port mapping \(this is referred to as *static* host port mapping\), or you can omit the `hostPort` \(or set it to `0`\) while specifying a `containerPort` and your container automatically receives a port \(this is referred to as *dynamic* host port mapping\) in the ephemeral port range for your container instance operating system and Docker version\.  
The default ephemeral port range Docker version 1\.6\.0 and later is listed on the instance under `/proc/sys/net/ipv4/ip_local_port_range`\. If this kernel parameter is unavailable, the default ephemeral port range from `49153–65535` is used\. Don't attempt to specify a host port in the ephemeral port range\. This is because these are reserved for automatic assignment\. In general, ports below `32768` are outside of the ephemeral port range\.   
The default reserved ports are `22` for SSH, the Docker ports `2375` and `2376`, and the Amazon ECS container agent ports `51678-51680`\. Any host port that was previously user\-specified for a running task is also reserved while the task is running \(after a task stops, the host port is released\)\. The current reserved ports are displayed in the `remainingResources` of describe\-container\-instances output, and a container instance might have up to 100 reserved ports at a time, including the default reserved ports\. Automatically assigned ports do not count toward the 100 reserved ports limit\.  
`name`  
Type: string  
Required: no, required for Service Connect to be configured in a service  
The name that's used for the port mapping\. This parameter only applies to Service Connect\. This parameter is the name that you use in the Service Connect configuration of a service\.  
For more information, see [Service Connect ](service-connect.md)\.  
In the following example, both of the required fields for Service Connect are shown\.  

```
"portMappings": [
    {
        "name": string,
        "containerPort": integer
    }
]
```  
`protocol`  
Type: string  
Required: no  
The protocol that's used for the port mapping\. Valid values are `tcp` and `udp`\. The default is `tcp`\.  
Only `tcp` is supported for Service Connect\. Remember that `tcp` is implied if this field isn't set\. 
UDP support is only available on container instances that were launched with version 1\.2\.0 of the Amazon ECS container agent \(such as the `amzn-ami-2015.03.c-amazon-ecs-optimized` AMI\) or later, or with container agents that have been updated to version 1\.3\.0 or later\. To update your container agent to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.
If you're specifying a host port, use the following syntax\.  

```
"portMappings": [
    {
        "containerPort": integer,
        "hostPort": integer
    }
    ...
]
```
If you want an automatically assigned host port, use the following syntax\.  

```
"portMappings": [
    {
        "containerPort": integer
    }
    ...
]
```

### Advanced container definition parameters<a name="advanced_container_definition_params"></a>

The following advanced container definition parameters provide extended capabilities to the [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/) command that's used to launch containers on your Amazon ECS container instances\.

**Topics**
+ [Health check](#container_definition_healthcheck)
+ [Environment](#container_definition_environment)
+ [Network settings](#container_definition_network)
+ [Storage and logging](#container_definition_storage)
+ [Security](#container_definition_security)
+ [Resource limits](#container_definition_limits)
+ [Docker labels](#container_definition_labels)

#### Health check<a name="container_definition_healthcheck"></a>

`healthCheck`  
The container health check command and the associated configuration parameters for the container\. This parameter maps to `HealthCheck` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `HEALTHCHECK` parameter of [docker run](https://docs.docker.com/engine/reference/run/)\.   
The Amazon ECS container agent only monitors and reports on the health checks that are specified in the task definition\. Amazon ECS doesn't monitor Docker health checks that are embedded in a container image but aren't specified in the container definition\. Health check parameters that are specified in a container definition override any Docker health checks that exist in the container image\.
You can view the health status of both individual containers and a task with the DescribeTasks API operation or when viewing the task details in the console\.  
The following describes the possible `healthStatus` values for a container:  
+ `HEALTHY`—The container health check has passed successfully\.
+ `UNHEALTHY`—The container health check has failed\.
+ `UNKNOWN`—The container health check is being evaluated or there's no container health check defined\.
The following describes the possible `healthStatus` values for a task\. The container health check status of non\-essential containers don't have an effect on the health status of a task\.  
+ `HEALTHY`—All essential containers within the task have passed their health checks\.
+ `UNHEALTHY`—One or more essential containers have failed their health check\.
+ `UNKNOWN`—The essential containers within the task are still having their health checks evaluated or there are no container health checks defined\.
If a task is run manually and not as part of a service, it continues its lifecycle regardless of its health status\. For tasks that are part of a service, if the task reports as unhealthy, then the task is stopped and the service scheduler replaces it\.  
The following are notes about container health check support:  
+ Container health checks require version 1\.17\.0 or greater of the Amazon ECS container agent\. For more information, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.
+ Container health checks are supported for Fargate tasks if you're using Linux platform version 1\.1\.0 or later\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.  
`command`  
A string array representing the command that the container runs to determine if it's healthy\. The string array can start with `CMD` to run the command arguments directly, or `CMD-SHELL` to run the command with the container's default shell\. If neither is specified, `CMD` is used\.  
When registering a task definition in the AWS Management Console, use a comma separated list of commands, which are converted to a string after the task definition is created\. An example input for a health check is the following\.  

```
CMD-SHELL, curl -f http://localhost/ || exit 1
```
When registering a task definition using the AWS Management Console JSON panel, the AWS CLI, or the APIs, you should enclose the list of commands in brackets\. An example input for a health check is the following\.  

```
[ "CMD-SHELL", "curl -f http://localhost/ || exit 1" ]
```
An exit code of 0, with no `stderr` output, indicates success, and a non\-zero exit code indicates failure\. For more information, see `HealthCheck` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/)\.  
`interval`  
The period of time \(in seconds\) between each health check\. You may specify between 5 and 300 seconds\. The default value is 30 seconds\.  
`timeout`  
The period of time \(in seconds\) to wait for a health check to succeed before it's considered a failure\. You may specify between 2 and 60 seconds\. The default value is 5 seconds\.  
`retries`  
The number of times to retry a failed health check before the container is considered unhealthy\. You may specify between 1 and 10 retries\. The default value is three retries\.  
`startPeriod`  
The optional grace period to provide containers time to bootstrap in before failed health checks count towards the maximum number of retries\. You can specify between 0 and 300 seconds\. By default, `startPeriod` is disabled\.

#### Environment<a name="container_definition_environment"></a>

`cpu`  
Type: integer  
Required: conditional  
The number of `cpu` units the Amazon ECS container agent reserves for the container\. On Linux, this parameter maps to `CpuShares` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--cpu-shares` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This field is optional for tasks that use the Fargate launch type\. The total amount of CPU reserved for all the containers that are within a task must be lower than the task\-level `cpu` value\.  
You can determine the number of CPU units that are available to each Amazon EC2 instance type by multiplying the number of vCPUs listed for that instance type on the [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/) detail page by 1,024\.
Linux containers share unallocated CPU units with other containers on the container instance with the same ratio as their allocated amount\. For example, assume that you run a single\-container task on a single\-core instance type with 512 CPU units specified for that container, and that task is the only task running on the container instance\. In this example, the container can use the full 1,024 CPU unit share at any given time\. However, assume then that you launched another copy of the same task on that container instance\. Each task is guaranteed a minimum of 512 CPU units when needed, and each container can float to higher CPU usage if the other container was not using it\. However, if both tasks were 100% active all of the time, they are limited to 512 CPU units\.  
On Linux container instances, the Docker daemon on the container instance uses the CPU value to calculate the relative CPU share ratios for running containers\. For more information, see [CPU share constraint](https://docs.docker.com/engine/reference/run/#cpu-share-constraint) in the Docker documentation\. The minimum valid CPU share value that the Linux kernel allows is 2\. However, the CPU parameter isn't required, and you can use CPU values below two in your container definitions\. For CPU values below two \(including null\), the behavior varies based on your Amazon ECS container agent version:  
+ **Agent versions <= 1\.1\.0:** Null and zero CPU values are passed to Docker as 0, which Docker then converts to 1,024 CPU shares\. CPU values of one are passed to Docker as one, which the Linux kernel converts to two CPU shares\.
+ **Agent versions >= 1\.2\.0:** Null, zero, and CPU values of one are passed to Docker as two CPU shares\.
On Windows container instances, the CPU limit is enforced as an absolute quota\. Windows containers only have access to the specified amount of CPU that's defined in the task definition\. A null or zero CPU value is passed to Docker as `0`, which Windows interprets as 1% of one CPU\.  
For additional examples, see [How Amazon ECS manages CPU and memory resources](http://aws.amazon.com/blogs/containers/how-amazon-ecs-manages-cpu-and-memory-resources/)\.

`gpu`  
Type: [ResourceRequirement](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ResourceRequirement.html) object  
Required: no  
The number of physical `GPUs` that the Amazon ECS container agent reserves for the container\. The number of GPUs reserved for all containers in a task must not exceed the number of available GPUs on the container instance the task is launched on\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.  
  
This parameter isn't supported for Windows containers or containers that are hosted on Fargate\.

`Elastic Inference accelerator`  
Type: [ResourceRequirement](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ResourceRequirement.html) object  
Required: no  
For the `InferenceAccelerator` type, the `value` matches the `deviceName` for an `InferenceAccelerator` specified in a task definition\. For more information, see [Elastic Inference accelerator name](#elastic-Inference-accelerator)\.  
This parameter is not supported for Windows containers or containers hosted on Fargate\.

`essential`  
Type: Boolean  
Required: no  
If the `essential` parameter of a container is marked as `true`, and that container fails or stops for any reason, all other containers that are part of the task are stopped\. If the `essential` parameter of a container is marked as `false`, then its failure doesn't affect the rest of the containers in a task\. If this parameter is omitted, a container is assumed to be essential\.  
All tasks must have at least one essential container\. If you have an application that's composed of multiple containers, group containers that are used for a common purpose into components, and separate the different components into multiple task definitions\. For more information, see [Application architecture](application_architecture.md)\.  

```
"essential": true|false
```

`entryPoint`  
Early versions of the Amazon ECS container agent don't properly handle `entryPoint` parameters\. If you have problems using `entryPoint`, update your container agent or enter your commands and arguments as `command` array items instead\.
Type: string array  
Required: no  
The entry point that's passed to the container\. This parameter maps to `Entrypoint` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--entrypoint` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. For more information about the Docker `ENTRYPOINT` parameter, see [https://docs\.docker\.com/engine/reference/builder/\#entrypoint](https://docs.docker.com/engine/reference/builder/#entrypoint)\.   

```
"entryPoint": ["string", ...]
```

`command`  
Type: string array  
Required: no  
The command that's passed to the container\. This parameter maps to `Cmd` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `COMMAND` parameter to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. For more information about the Docker `CMD` parameter, see [https://docs\.docker\.com/engine/reference/builder/\#cmd](https://docs.docker.com/engine/reference/builder/#cmd)\. If there are multiple arguments,make sure that each argument is a separated string in the array\.  

```
"command": ["string", ...]
```

`workingDirectory`  
Type: string  
Required: no  
The working directory to run commands inside the container in\. This parameter maps to `WorkingDir` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--workdir` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  

```
"workingDirectory": "string"
```

`environmentFiles`  
Type: object array  
Required: no  
A list of files containing the environment variables to pass to a container\. This parameter maps to the `--env-file` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This is not available for Windows containers\.  
You can specify up to ten environment files\. The file must have a `.env` file extension\. Each line in an environment file contains an environment variable in `VARIABLE=VALUE` format\. Lines beginning with `#` are treated as comments and are ignored\. For more information about the appropriate environment variable file syntax, see [Declare default environment variables in file](https://docs.docker.com/compose/env-file/)\.  
If there are individual environment variables specified in the container definition, they take precedence over the variables contained within an environment file\. If multiple environment files are specified that contain the same variable, they're processed from the top down\. We recommend that you use unique variable names\. For more information, see [Passing environment variables to a container](taskdef-envfiles.md)\.    
`value`  
Type: String  
Required: Yes  
The Amazon Resource Name \(ARN\) of the Amazon S3 object containing the environment variable file\.  
`type`  
Type: String  
Required: Yes  
The file type to use\. The only supported value is `s3`\.

`environment`  
Type: object array  
Required: no  
The environment variables to pass to a container\. This parameter maps to `Env` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--env` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.  
`name`  
Type: String  
Required: Yes, when `environment` is used  
The name of the environment variable\.  
`value`  
Type: String  
Required: Yes, when `environment` is used  
The value of the environment variable\.

```
"environment" : [
    { "name" : "string", "value" : "string" },
    { "name" : "string", "value" : "string" }
]
```

`secrets`  
Type: Object array  
Required: No  
An object representing the secret to expose to your container\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.    
`name`  
Type: String  
Required: Yes  
The value to set as the environment variable on the container\.  
`valueFrom`  
Type: String  
Required: Yes  
The secret to expose to the container\. The supported values are either the full Amazon Resource Name \(ARN\) of the AWS Secrets Manager secret or the full ARN of the parameter in the AWS Systems Manager Parameter Store\.  
If the Systems Manager Parameter Store parameter exists in the same AWS Region as the task that you're launching, you can use either the full ARN or name of the secret\. If the parameter exists in a different Region then the full ARN must be specified\.

```
"secrets": [
    {
        "name": "environment_variable_name",
        "valueFrom": "arn:aws:ssm:region:aws_account_id:parameter/parameter_name"
    }
]
```

#### Network settings<a name="container_definition_network"></a>

`disableNetworking`  
Type: Boolean  
Required: no  
When this parameter is true, networking is off within the container\. This parameter maps to `NetworkDisabled` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/)\.  
This parameter isn't supported for Windows containers or tasks using the `awsvpc` network mode\.

```
"disableNetworking": true|false
```

`links`  
Type: string array  
Required: no  
The `link` parameter allows containers to communicate with each other without the need for port mappings\. Only supported if the network mode of a task definition is set to `bridge`\. The `name:internalName` construct is analogous to `name:alias` in Docker links\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. For more information about linking Docker containers, see [https://docs\.docker\.com/engine/userguide/networking/default\_network/dockerlinks/](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)\. This parameter maps to `Links` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--link` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter isn't supported for Windows containers or tasks using the `awsvpc` network mode\.
Containers that are collocated on the same container instance might be able to communicate with each other without requiring links or host port mappings\. The network isolation on a container instance is controlled by security groups and VPC settings\.

```
"links": ["name:internalName", ...]
```

`hostname`  
Type: string  
Required: no  
The hostname to use for your container\. This parameter maps to `Hostname` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--hostname` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
If you're using the `awsvpc` network mode, the `hostname` parameter isn't supported\.

```
"hostname": "string"
```

`dnsServers`  
Type: string array  
Required: no  
A list of DNS servers that are presented to the container\. This parameter maps to `Dns` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--dns` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter isn't supported for Windows containers or tasks using the `awsvpc` network mode\.

```
"dnsServers": ["string", ...]
```

`dnsSearchDomains`  
Type: string array  
Required: no  
Pattern: ^\[a\-zA\-Z0\-9\-\.\]\{0,253\}\[a\-zA\-Z0\-9\]$  
A list of DNS search domains that are presented to the container\. This parameter maps to `DnsSearch` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--dns-search` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter isn't supported for Windows containers or tasks that use the `awsvpc` network mode\.

```
"dnsSearchDomains": ["string", ...]
```

`extraHosts`  
Type: object array  
Required: no  
A list of hostnames and IP address mappings to append to the `/etc/hosts` file on the container\.   
This parameter maps to `ExtraHosts` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--add-host` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter isn't supported for Windows containers or tasks that use the `awsvpc` network mode\.

```
"extraHosts": [
      {
        "hostname": "string",
        "ipAddress": "string"
      }
      ...
    ]
```  
`hostname`  
Type: string  
Required: yes, when `extraHosts` are used  
The hostname to use in the `/etc/hosts` entry\.  
`ipAddress`  
Type: string  
Required: yes, when `extraHosts` are used  
The IP address to use in the `/etc/hosts` entry\.

#### Storage and logging<a name="container_definition_storage"></a>

`readonlyRootFilesystem`  
Type: Boolean  
Required: no  
When this parameter is true, the container is given read\-only access to its root file system\. This parameter maps to `ReadonlyRootfs` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--read-only` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

```
"readonlyRootFilesystem": true|false
```

`mountPoints`  
Type: Object Array  
Required: No  
The mount points for data volumes in your container\.   
This parameter maps to `Volumes` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--volume` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers cannot mount directories on a different drive, and mount point cannot be across drives\.    
`sourceVolume`  
Type: String  
Required: Yes, when `mountPoints` are used  
The name of the volume to mount\.  
`containerPath`  
Type: String  
Required: Yes, when `mountPoints` are used  
The path on the container to mount the volume at\.  
`readOnly`  
Type: Boolean  
Required: No  
If this value is `true`, the container has read\-only access to the volume\. If this value is `false`, then the container can write to the volume\. The default value is `false`\.

`volumesFrom`  
Type: Object Array  
Required: No  
Data volumes to mount from another container\. This parameter maps to `VolumesFrom` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--volumes-from` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.    
`sourceContainer`  
Type: string  
Required: yes, when `volumesFrom` is used  
The name of the container to mount volumes from\.  
`readOnly`  
Type: Boolean  
Required: no  
If this value is `true`, the container has read\-only access to the volume\. If this value is `false`, then the container can write to the volume\. The default value is `false`\.

```
"volumesFrom": [
                {
                  "sourceContainer": "string",
                  "readOnly": true|false
                }
              ]
```

`logConfiguration`  
Type: [LogConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LogConfiguration.html) Object  
Required: no  
The log configuration specification for the container\.  
For example task definitions that use a log configuration, see [Example task definitions](example_task_definitions.md)\.  
This parameter maps to `LogConfig` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--log-driver` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. By default, containers use the same logging driver that the Docker daemon uses\. However, the container might use a different logging driver than the Docker daemon by specifying a log driver with this parameter in the container definition\. To use a different logging driver for a container, the log system must be configured properly on the container instance \(or on a different log server for remote logging options\)\. For more information about the options for different supported log drivers, see [Configure logging drivers](https://docs.docker.com/engine/admin/logging/overview/) in the Docker documentation\.  
Consider the following when specifying a log configuration for your containers:  
+ Amazon ECS supports a subset of the logging drivers that are available to the Docker daemon \(shown in the valid values that follow\)\. Additional log drivers might be available in future releases of the Amazon ECS container agent\.
+ This parameter requires version 1\.18 or later of the Docker Remote API on your container instance\.
+ For tasks that use the EC2 launch type, the Amazon ECS container agent that runs on a container instance must register the logging drivers that are available on that instance with the `ECS_AVAILABLE_LOGGING_DRIVERS` environment variable before containers placed on that instance can use these log configuration options\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.
+ For tasks that use the Fargate launch type, because you don't have access to the underlying infrastructure your tasks are hosted on, any additional software needed must be installed outside of the task\. For example, the Fluentd output aggregators or a remote host running Logstash to send Gelf logs to\.

```
"logConfiguration": {
      "logDriver": "awslogs","fluentd","gelf","json-file","journald","logentries","splunk","syslog","awsfirelens",
      "options": {"string": "string"
        ...},
	"secretOptions": [{
		"name": "string",
		"valueFrom": "string"
	}]
}
```  
`logDriver`  
Type: string  
Valid values: `"awslogs","fluentd","gelf","json-file","journald","logentries","splunk","syslog","awsfirelens"`  
Required: yes, when `logConfiguration` is used  
The log driver to use for the container\. By default, the valid values that are listed earlier are log drivers that the Amazon ECS container agent can communicate with\.  
For tasks that use the Fargate launch type, the supported log drivers are `awslogs`, `splunk`, and `awsfirelens`\.  
For tasks that use the EC2 launch type, the supported log drivers are `awslogs`, `fluentd`, `gelf`, `json-file`, `journald`, `logentries`,`syslog`, `splunk`, and `awsfirelens`\.  
For more information about how to use the `awslogs` log driver in task definitions to send your container logs to CloudWatch Logs, see [Using the awslogs log driver](using_awslogs.md)\.  
For more information about using the `awsfirelens` log driver, see [Custom Log Routing](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_firelens.html)\.  
If you have a custom driver that isn't listed, you can fork the Amazon ECS container agent project that's [available on GitHub](https://github.com/aws/amazon-ecs-agent) and customize it to work with that driver\. We encourage you to submit pull requests for changes that you want to have included\. However, we don't currently provide support for running modified copies of this software\.
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  
`options`  
Type: string to string map  
Required: no  
The configuration options to send to the log driver\.  
When you use FireLens to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics, you can set the `log-driver-buffer-limit` option to limit the number of events that are buffered in memory, before being sent to the log router container\. It can help to resolve potential log loss issue because high throughput might result in memory running out for the buffer inside of Docker\. For more information, see [Fluentd buffer limit](using_firelens.md#firelens-docker-buffer-limit)\.  
This parameter requires version 1\.19 of the Docker Remote API or greater on your container instance\.  
`secretOptions`  
Type: object array  
Required: no  
An object that represents the secret to pass to the log configuration\. Secrets used in log configuration may include an authentication token, certificate, or encryption key, for example\.\) For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.    
`name`  
Type: String  
Required: Yes  
The value to set as the environment variable on the container\.  
`valueFrom`  
Type: String  
Required: Yes  
The secret to expose to the log configuration of the container\.

```
"logConfiguration": {
	"logDriver": "splunk",
	"options": {
		"splunk-url": "https://cloud.splunk.com:8080",
		"splunk-token": "...",
		"tag": "...",
		...
	},
	"secretOptions": [{
		"name": "splunk-token",
		"valueFrom": "/ecs/logconfig/splunkcred"
	}]
}
```

`firelensConfiguration`  
Type: [FirelensConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_FirelensConfiguration.html) Object  
Required: No  
The FireLens configuration for the container\. This is used to specify and configure a log router for container logs\. For more information, see [Custom log routing](using_firelens.md)\.  

```
{
    "firelensConfiguration": {
        "type": "fluentd",
        "options": {
            "KeyName": ""
        }
    }
}
```  
`options`  
Type: String to string map  
Required: No  
The options to use when configuring the log router\. This field is optional and can be used to specify a custom configuration file or to add additional metadata, such as the task, task definition, cluster, and container instance details to the log event\. If specified, the syntax to use is `"options":{"enable-ecs-log-metadata":"true|false","config-file-type:"s3|file","config-file-value":"arn:aws:s3:::mybucket/fluent.conf|filepath"}`\. For more information, see [Creating a task definition that uses a FireLens configuration](firelens-taskdef.md)\.  
`type`  
Type: String  
Required: Yes  
The log router to use\. The valid values are `fluentd` or `fluentbit`\.

#### Security<a name="container_definition_security"></a>

For more information about container security, see [Task and container security](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/security-tasks-containers.html) in the *Amazon ECS Best Practices Guide*\.

`privileged`  
Type: Boolean  
Required: no  
When this parameter is true, the container is given elevated privileges on the host container instance \(similar to the `root` user\)\. We recommend against running containers with `privileged`\. In most cases, you can specify the exact privileges that you need by using the specific parameters instead of using `privileged`\.  
This parameter maps to `Privileged` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--privileged` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.

```
"privileged": true|false
```

`user`  
Type: string  
Required: no  
The user to use inside the container\. This parameter maps to `User` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--user` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
When running tasks that use the `host` network mode, do not run containers using the root user \(UID 0\)\. As a security best practice, always use a non\-root user\.
You can specify the `user` using the following formats\. If specifying a UID or GID, you must specify it as a positive integer\.  
+ `user`
+ `user:group`
+ `uid`
+ `uid:gid`
+ `user:gid`
+ `uid:group`
This parameter is not supported for Windows containers\.

```
"user": "string"
```

`dockerSecurityOptions`  
Type: string array  
Valid values: "no\-new\-privileges" \| "apparmor:PROFILE" \| "label:*value*" \| "credentialspec:*CredentialSpecFilePath*"  
Required: no  
A list of strings to provide custom labels for SELinux and AppArmor multi\-level security systems\. For more information about valid values, see [Docker Run Security Configuration](https://docs.docker.com/engine/reference/run/#security-configuration)\. This field isn't valid for containers in tasks using the Fargate launch type\.  
With Windows containers, this parameter can be used to reference a credential spec file when configuring a container for Active Directory authentication\. For more information, see [Using gMSAs for Windows Containers](windows-gmsa.md)\.  
This parameter maps to `SecurityOpt` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--security-opt` option to [https://docs.docker.com/engine/reference/run/#security-configuration](https://docs.docker.com/engine/reference/run/#security-configuration)\.  

```
"dockerSecurityOptions": ["string", ...]
```
The Amazon ECS container agent that run on a container instance must register with the `ECS_SELINUX_CAPABLE=true` or `ECS_APPARMOR_CAPABLE=true` environment variables before containers that are placed on that instance can use these security options\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

#### Resource limits<a name="container_definition_limits"></a>

`ulimits`  
Type: object array  
Required: no  
A list of `ulimit` values to define for a container\. This value overwrites the default resource quota setting for the operating system\. This parameter maps to `Ulimits` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--ulimit` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
Amazon ECS tasks hosted on Fargate use the default resource limit values set by the operating system with the exception of the `nofile` resource limit parameter which Fargate overrides\. The `nofile` resource limit sets a restriction on the number of open files that a container can use\. The default `nofile` soft limit is `1024` and hard limit is `4096`\. For more information, see [Task resource limits](AWS_Fargate.md#fargate-resource-limits)\.  
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  
This parameter is not supported for Windows containers\.

```
"ulimits": [
      {
        "name": "core"|"cpu"|"data"|"fsize"|"locks"|"memlock"|"msgqueue"|"nice"|"nofile"|"nproc"|"rss"|"rtprio"|"rttime"|"sigpending"|"stack",
        "softLimit": integer,
        "hardLimit": integer
      }
      ...
    ]
```  
`name`  
Type: string  
Valid values: `"core" | "cpu" | "data" | "fsize" | "locks" | "memlock" | "msgqueue" | "nice" | "nofile" | "nproc" | "rss" | "rtprio" | "rttime" | "sigpending" | "stack"`  
Required: yes, when `ulimits` are used  
The `type` of the `ulimit`\.  
`hardLimit`  
Type: integer  
Required: yes, when `ulimits` are used  
The hard limit for the `ulimit` type\.  
`softLimit`  
Type: integer  
Required: yes, when `ulimits` are used  
The soft limit for the `ulimit` type\.

#### Docker labels<a name="container_definition_labels"></a>

`dockerLabels`  
Type: string to string map  
Required: no  
A key/value map of labels to add to the container\. This parameter maps to `Labels` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--label` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.   
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  

```
"dockerLabels": {"string": "string"
      ...}
```

### Other container definition parameters<a name="other_container_definition_params"></a>

The following container definition parameters can be used when registering task definitions in the Amazon ECS console by using the **Configure via JSON** option\. For more information, see [Creating a task definition using the console](create-task-definition.md)\.

**Topics**
+ [Linux parameters](#container_definition_linuxparameters)
+ [Container dependency](#container_definition_dependson)
+ [Container timeouts](#container_definition_timeout)
+ [System controls](#container_definition_systemcontrols)
+ [Interactive](#container_definition_interactive)
+ [Pseudo terminal](#container_definition_pseudoterminal)

#### Linux parameters<a name="container_definition_linuxparameters"></a>

`linuxParameters`  
Type: [LinuxParameters](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LinuxParameters.html) object  
Required: no  
Linux\-specific options that are applied to the container, such as [KernelCapabilities](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_KernelCapabilities.html)\.  
This parameter isn't supported for Windows containers\.

```
"linuxParameters": {
      "capabilities": {
        "add": ["string", ...],
        "drop": ["string", ...]
        }
      }
```  
`capabilities`  
Type: [KernelCapabilities](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_KernelCapabilities.html) object  
Required: no  
The Linux capabilities for the container that are added to or dropped from the default configuration provided by Docker\. For more information about the default capabilities and the other available capabilities, see [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) in the *Docker run reference*\. For more information about these Linux capabilities, see the [capabilities\(7\)](http://man7.org/linux/man-pages/man7/capabilities.7.html) Linux manual page\.    
`add`  
Type: string array  
Valid values: `"ALL" | "AUDIT_CONTROL" | "AUDIT_READ" | "AUDIT_WRITE" | "BLOCK_SUSPEND" | "CHOWN" | "DAC_OVERRIDE" | "DAC_READ_SEARCH" | "FOWNER" | "FSETID" | "IPC_LOCK" | "IPC_OWNER" | "KILL" | "LEASE" | "LINUX_IMMUTABLE" | "MAC_ADMIN" | "MAC_OVERRIDE" | "MKNOD" | "NET_ADMIN" | "NET_BIND_SERVICE" | "NET_BROADCAST" | "NET_RAW" | "SETFCAP" | "SETGID" | "SETPCAP" | "SETUID" | "SYS_ADMIN" | "SYS_BOOT" | "SYS_CHROOT" | "SYS_MODULE" | "SYS_NICE" | "SYS_PACCT" | "SYS_PTRACE" | "SYS_RAWIO" | "SYS_RESOURCE" | "SYS_TIME" | "SYS_TTY_CONFIG" | "SYSLOG" | "WAKE_ALARM"`  
Required: no  
The Linux capabilities for the container to add to the default configuration provided by Docker\. This parameter maps to `CapAdd` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--cap-add` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
Tasks that are launched on Fargate only support adding the `SYS_PTRACE` kernel capability\.  
`drop`  
Type: string array  
Valid values: `"ALL" | "AUDIT_CONTROL" | "AUDIT_WRITE" | "BLOCK_SUSPEND" | "CHOWN" | "DAC_OVERRIDE" | "DAC_READ_SEARCH" | "FOWNER" | "FSETID" | "IPC_LOCK" | "IPC_OWNER" | "KILL" | "LEASE" | "LINUX_IMMUTABLE" | "MAC_ADMIN" | "MAC_OVERRIDE" | "MKNOD" | "NET_ADMIN" | "NET_BIND_SERVICE" | "NET_BROADCAST" | "NET_RAW" | "SETFCAP" | "SETGID" | "SETPCAP" | "SETUID" | "SYS_ADMIN" | "SYS_BOOT" | "SYS_CHROOT" | "SYS_MODULE" | "SYS_NICE" | "SYS_PACCT" | "SYS_PTRACE" | "SYS_RAWIO" | "SYS_RESOURCE" | "SYS_TIME" | "SYS_TTY_CONFIG" | "SYSLOG" | "WAKE_ALARM"`  
Required: no  
The Linux capabilities for the container to remove from the default configuration that's provided by Docker\. This parameter maps to `CapDrop` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--cap-drop` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
`devices`  
Any host devices to expose to the container\. This parameter maps to `Devices` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--device` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you're using tasks that use the Fargate launch type, the `devices` parameter isn't supported\.
Type: Array of [Device](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Device.html) objects  
Required: No    
`hostPath`  
The path for the device on the host container instance\.  
Type: String  
Required: Yes  
`containerPath`  
The path inside the container to expose the host device at\.  
Type: String  
Required: No  
`permissions`  
The explicit permissions to provide to the container for the device\. By default, the container has permissions for `read`, `write`, and `mknod` on the device\.  
Type: Array of strings  
Valid Values: `read` \| `write` \| `mknod`  
`initProcessEnabled`  
Run an `init` process inside the container that forwards signals and reaps processes\. This parameter maps to the `--init` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
This parameter requires version 1\.25 of the Docker Remote API or greater on your container instance\.  
`maxSwap`  
The total amount of swap memory \(in MiB\) a container can use\. This parameter is translated to the `--memory-swap` option to [docker run](https://docs.docker.com/engine/reference/run/) where the value is the sum of the container memory plus the `maxSwap` value\.  
If a `maxSwap` value of `0` is specified, the container doesn't use swap\. Accepted values are `0` or any positive integer\. If the `maxSwap` parameter is omitted, the container uses the swap configuration for the container instance that it's running on\. A `maxSwap` value must be set for the `swappiness` parameter to be used\.  
If you're using tasks that use the Fargate launch type, the `maxSwap` parameter isn't supported\.  
`sharedMemorySize`  
The value for the size \(in MiB\) of the `/dev/shm` volume\. This parameter maps to the `--shm-size` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you are using tasks that use the Fargate launch type, the `sharedMemorySize` parameter is not supported\.
Type: Integer  
`swappiness`  
This allows you to tune a container's memory swappiness behavior\. A `swappiness` value of `0` prevents swapping from happening unless required\. A `swappiness` value of `100` causes pages to be swapped frequently\. Accepted values are whole numbers between `0` and `100`\. If you don't specify a value, the default value of `60` is used\. Moreover, if you don't specify a value for `maxSwap`, then this parameter is ignored\. This parameter maps to the `--memory-swappiness` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you're using tasks that use the Fargate launch type, the `swappiness` parameter isn't supported\.  
`tmpfs`  
The container path, mount options, and maximum size \(in MiB\) of the tmpfs mount\. This parameter maps to the `--tmpfs` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you're using tasks that use the Fargate launch type, the `tmpfs` parameter isn't supported\.
Type: Array of [Tmpfs](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Tmpfs.html) objects  
Required: No    
`containerPath`  
The absolute file path where the tmpfs volume is to be mounted\.  
Type: String  
Required: Yes  
`mountOptions`  
The list of tmpfs volume mount options\.  
Type: Array of strings  
Required: No  
Valid Values: `"defaults" | "ro" | "rw" | "suid" | "nosuid" | "dev" | "nodev" | "exec" | "noexec" | "sync" | "async" | "dirsync" | "remount" | "mand" | "nomand" | "atime" | "noatime" | "diratime" | "nodiratime" | "bind" | "rbind" | "unbindable" | "runbindable" | "private" | "rprivate" | "shared" | "rshared" | "slave" | "rslave" | "relatime" | "norelatime" | "strictatime" | "nostrictatime" | "mode" | "uid" | "gid" | "nr_inodes" | "nr_blocks" | "mpol"`  
`size`  
The maximum size \(in MiB\) of the tmpfs volume\.  
Type: Integer  
Required: Yes

#### Container dependency<a name="container_definition_dependson"></a>

`dependsOn`  
Type: Array of [ContainerDependency](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerDependency.html) objects  
Required: no  
The dependencies defined for container startup and shutdown\. A container can contain multiple dependencies\. When a dependency is defined for container startup, for container shutdown it is reversed\. For an example, see [Example: Container dependency](example_task_definitions.md#example_task_definition-containerdependency)\.  
If a container doesn't meet a dependency constraint or times out before meeting the constraint, Amazon ECS doesn't progress dependent containers to their next state\.
For Amazon ECS tasks that are hosted on Amazon EC2 instances, the instances require at least version `1.26.0` of the container agent to enable container dependencies\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\. If you're using an Amazon ECS\-optimized Amazon Linux AMI, your instance needs at least version `1.26.0-1` of the `ecs-init` package\. If your container instances are launched from version `20190301` or later, they contain the required versions of the container agent and `ecs-init`\. For more information, see [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)\.  
For Amazon ECS tasks that are hosted on Fargate, this parameter requires that the task or service uses platform version `1.3.0` or later \(Linux\) or `1.0.0` \(Windows\)\.  

```
"dependsOn": [
    {
        "containerName": "string",
        "condition": "string"
    }
]
```  
`containerName`  
Type: String  
Required: Yes  
The container name that must meet the specified condition\.  
`condition`  
Type: String  
Required: Yes  
The dependency condition of the container\. The following are the available conditions and their behavior:  
+ `START` – This condition emulates the behavior of links and volumes today\. It validates that a dependent container is started before permitting other containers to start\.
+ `COMPLETE` – This condition validates that a dependent container runs to completion \(exits\) before permitting other containers to start\. This can be useful for non\-essential containers that run a script and then exit\. This condition can't be set on an essential container\.
+ `SUCCESS` – This condition is the same as `COMPLETE`, but it also requires that the container exits with a `zero` status\. This condition can't be set on an essential container\.
+ `HEALTHY` – This condition validates that the dependent container passes its container health check before permitting other containers to start\. This requires that the dependent container has health checks configured in the task definition\. This condition is confirmed only at task startup\.

#### Container timeouts<a name="container_definition_timeout"></a>

`startTimeout`  
Type: Integer  
Required: no  
Example values: `120`  
Time duration \(in seconds\) to wait before giving up on resolving dependencies for a container\.  
For example, you specify two containers in a task definition with `containerA` having a dependency on `containerB` reaching a `COMPLETE`, `SUCCESS`, or `HEALTHY` status\. If a `startTimeout` value is specified for `containerB` and it doesn't reach the desired status within that time, then `containerA` doesn't start\.  
If a container doesn't meet a dependency constraint or times out before meeting the constraint, Amazon ECS doesn't progress dependent containers to their next state\.
For Amazon ECS tasks that are hosted on Fargate, this parameter requires that the task or service uses platform version `1.3.0` or later \(Linux\)\.

`stopTimeout`  
Type: Integer  
Required: no  
Example values: `120`  
Time duration \(in seconds\) to wait before the container is forcefully killed if it doesn't exit normally on its own\.  
For tasks that use the Fargate launch type, the task or service requires platform version 1\.3\.0 or later \(Linux\) or 1\.0\.0 or later \(for Windows\)\. The max stop timeout value is 120 seconds\. However, if the parameter isn't specified, the default value of 30 seconds is used\.  
For tasks that use the EC2 launch type, if the `stopTimeout` parameter isn't specified, the value set for the Amazon ECS container agent configuration variable `ECS_CONTAINER_STOP_TIMEOUT` is used\. If neither the `stopTimeout` parameter or the `ECS_CONTAINER_STOP_TIMEOUT` agent configuration variable is set, the default values of 30 seconds for Linux containers and 30 seconds on Windows containers are used\. Container instances require at least version 1\.26\.0 of the container agent to enable a container stop timeout value\. However, we recommend using the latest container agent version\. For information about how to check your agent version and update to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\. If you're using an Amazon ECS\-optimized Amazon Linux AMI, your instance needs at least version 1\.26\.0\-1 of the `ecs-init` package\. If your container instances are launched from version `20190301` or later, they contain the required versions of the container agent and `ecs-init`\. For more information, see [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)\.

#### System controls<a name="container_definition_systemcontrols"></a>

`systemControls`  
Type: [SystemControl](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_SystemControl.html) object  
Required: no  
A list of namespaced kernel parameters to set in the container\. This parameter maps to `Sysctls` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--sysctl` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
We do not recommend that you specify network\-related `systemControls` parameters for multiple containers in a single task that also uses either the `awsvpc` or `host` network mode for the following reasons:  
+ For tasks that use the `awsvpc` network mode, if you set `systemControls` for any container, it applies to all containers in the task\. If you set different `systemControls` for multiple containers in a single task, the container that's started last determines which `systemControls` take effect\.
+ For tasks that use the `host` network mode, the network namespace `systemControls` aren't supported\.
If you're setting an IPC resource namespace to use for the containers in the task, the following conditions apply to your system controls\. For more information, see [IPC mode](#task_definition_ipcmode)\.  
+ For tasks that use the `host` IPC mode, IPC namespace `systemControls` aren't supported\.
+ For tasks that use the `task` IPC mode, IPC namespace `systemControls` values applies to all containers within a task\.
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.

```
"systemControls": [
    {
         "namespace":"string",
         "value":"string"
    }
]
```  
`namespace`  
Type: String  
Required: no  
The namespaced kernel parameter to set a `value` for\.  
Valid IPC namespace values: `"kernel.msgmax" | "kernel.msgmnb" | "kernel.msgmni" | "kernel.sem" | "kernel.shmall" | "kernel.shmmax" | "kernel.shmmni" | "kernel.shm_rmid_forced"`, as well as Sysctls beginning with `"fs.mqueue.*"`  
Valid network namespace values: Sysctls beginning with `"net.*"`  
`value`  
Type: String  
Required: no  
The value for the namespaced kernel parameter that's specified in `namespace`\.

#### Interactive<a name="container_definition_interactive"></a>

`interactive`  
Type: Boolean  
Required: no  
When this parameter is `true`, you can deploy containerized applications that require stdin or a tty to be allocated\. This parameter maps to `OpenStdin` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--interactive` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.

#### Pseudo terminal<a name="container_definition_pseudoterminal"></a>

`pseudoTerminal`  
Type: Boolean  
Required: no  
When this parameter is `true`, a TTY is allocated\. This parameter maps to `Tty` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--tty` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.

## Elastic Inference accelerator name<a name="elastic-Inference-accelerator"></a>

The Elastic Inference accelerator resource requirement for your task definition\. For more information, see [What Is Elastic Inference?](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html) in the Amazon Elastic Inference Developer Guide\.

The following parameters are allowed in a task definition:

`deviceName`  
Type: string  
Required: yes  
The Elastic Inference accelerator device name\. The `deviceName` must also be referenced in a container definition see [Elastic Inference accelerator](#ContainerDefinition-elastic-inference)\.

`deviceType`  
Type: string  
Required: yes  
The Elastic Inference accelerator to use\.

## Task placement constraints<a name="constraints"></a>

When you register a task definition, you can provide task placement constraints that customize how Amazon ECS places tasks\.

If you're using the Fargate launch type, task placement constraints aren't supported\. By default Fargate tasks are spread across Availability Zones\.

For tasks that use the EC2 launch type, you can use constraints to place tasks based on Availability Zone, instance type, or custom attributes\. For more information, see [Amazon ECS task placement constraints](task-placement-constraints.md)\.

The following parameters are allowed in a container definition:

`expression`  
Type: string  
Required: no  
A cluster query language expression to apply to the constraint\. For more information, see [Cluster query language](cluster-query-language.md)\.

`type`  
Type: string  
Required: yes  
The type of constraint\. Use `memberOf` to restrict the selection to a group of valid candidates\.

## Proxy configuration<a name="proxyConfiguration"></a>

`proxyConfiguration`  
Type: [ProxyConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ProxyConfiguration.html) object  
Required: no  
The configuration details for the App Mesh proxy\.  
For tasks that use the EC2 launch type, the container instances require at least version 1\.26\.0 of the container agent and at least version 1\.26\.0\-1 of the `ecs-init` package to enable a proxy configuration\. If your container instances are launched from the Amazon ECS\-optimized AMI version `20190301` or later, then they contain the required versions of the container agent and `ecs-init`\. For more information, see [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)\.  
For tasks that use the Fargate launch type, this feature requires that the task or service uses platform version 1\.3\.0 or later\.  
This parameter is not supported for Windows containers\.

```
"proxyConfiguration": {
    "type": "APPMESH",
    "containerName": "string",
    "properties": [
        {
           "name": "string",
           "value": "string"
        }
    ]
}
```  
`type`  
Type: String  
Value values: `APPMESH`  
Required: No  
The proxy type\. The only supported value is `APPMESH`\.  
`containerName`  
Type: String  
Required: Yes  
The name of the container that serves as the App Mesh proxy\.  
`properties`  
Type: Array of [KeyValuePair](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_KeyValuePair.html) objects  
Required: No  
The set of network configuration parameters to provide the Container Network Interface \(CNI\) plugin, specified as key\-value pairs\.  
+ `IgnoredUID` – \(Required\) The user ID \(UID\) of the proxy container as defined by the `user` parameter in a container definition\. This is used to ensure the proxy ignores its own traffic\. If `IgnoredGID` is specified, this field can be empty\.
+ `IgnoredGID` – \(Required\) The group ID \(GID\) of the proxy container as defined by the `user` parameter in a container definition\. This is used to ensure the proxy ignores its own traffic\. If `IgnoredUID` is specified, this field can be empty\.
+ `AppPorts` – \(Required\) The list of ports that the application uses\. Network traffic to these ports is forwarded to the `ProxyIngressPort` and `ProxyEgressPort`\.
+ `ProxyIngressPort` – \(Required\) Specifies the port that incoming traffic to the `AppPorts` is directed to\.
+ `ProxyEgressPort` – \(Required\) Specifies the port that outgoing traffic from the `AppPorts` is directed to\.
+ `EgressIgnoredPorts` – \(Required\) The outbound traffic going to these specified ports is ignored and not redirected to the `ProxyEgressPort`\. It can be an empty list\.
+ `EgressIgnoredIPs` – \(Required\) The outbound traffic going to these specified IP addresses is ignored and not redirected to the `ProxyEgressPort`\. It can be an empty list\.  
`name`  
Type: String  
Required: No  
The name of the key\-value pair\.  
`value`  
Type: String  
Required: No  
The value of the key\-value pair\.

## Volumes<a name="volumes"></a>

When you register a task definition, you can optionally specify a list of volumes to be passed to the Docker daemon on a container instance, which then becomes available for access by other containers on the same container instance\.

The following are the types of data volumes that can be used:
+ Docker volumes — A Docker\-managed volume that is created under `/var/lib/docker/volumes` on the host Amazon EC2 instance\. Docker volume drivers \(also referred to as plugins\) are used to integrate the volumes with external storage systems, such as Amazon EBS\. The built\-in `local` volume driver or a third\-party volume driver can be used\. Docker volumes are only supported when running tasks on Amazon EC2 instances\. Windows containers only support the use of the `local` driver\. To use Docker volumes, specify a `dockerVolumeConfiguration` in your task definition\. For more information, see [Using volumes](https://docs.docker.com/storage/volumes/)\.
+ Bind mounts — A file or directory on the host machine is mounted into a container\. Bind mount host volumes are supported when running tasks on either AWS Fargate or Amazon EC2 instances\. To use bind mount host volumes, specify a `host` and optional `sourcePath` value in your task definition\. For more information, see [Using bind mounts](https://docs.docker.com/storage/bind-mounts/)\.

For more information, see [Using data volumes in tasks](using_data_volumes.md)\.

The following parameters are allowed in a container definition\.

`name`  
Type: String  
Required: No  
The name of the volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. This name is referenced in the `sourceVolume` parameter of container definition `mountPoints` object\.

`host`  
Required: No  
The `host` parameter is only supported when using tasks hosted on Amazon EC2 instances\.
The `host` parameter is used to tie the lifecycle of the bind mount to the host Amazon EC2 instance, rather than the task, and where it is stored\. If the `host` parameter is empty, then the Docker daemon assigns a host path for your data volume, but the data is not guaranteed to persist after the containers associated with it stop running\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\.    
`sourcePath`  
Type: String  
Required: No  
When the `host` parameter is used, specify a `sourcePath` to declare the path on the host Amazon EC2 instance that is presented to the container\. If this parameter is empty, then the Docker daemon assigns a host path for you\. If the `host` parameter contains a `sourcePath` file location, then the data volume persists at the specified location on the host Amazon EC2 instance until you delete it manually\. If the `sourcePath` value does not exist on the host Amazon EC2 instance, the Docker daemon creates it\. If the location does exist, the contents of the source path folder are exported\.

`dockerVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when using Docker volumes\. Docker volumes are only supported when running tasks on EC2 instances\. Windows containers only support the use of the `local` driver\. To use bind mounts, specify a `host` instead\.    
`scope`  
Type: String  
Valid Values: `task` \| `shared`  
Required: No  
The scope for the Docker volume, which determines its lifecycle\. Docker volumes that are scoped to a `task` are automatically provisioned when the task starts and destroyed when the task stops\. Docker volumes that are scoped as `shared` persist after the task stops\.  
`autoprovision`  
Type: Boolean  
Default value: `false`  
Required: No  
If this value is `true`, the Docker volume is created if it does not already exist\. This field is only used if the `scope` is `shared`\. If the `scope` is `task` then this parameter must either be omitted or set to `false`\.  
`driver`  
Type: String  
Required: No  
The Docker volume driver to use\. The driver value must match the driver name provided by Docker because it is used for task placement\. If the driver was installed using the Docker plugin CLI, use `docker plugin ls` to retrieve the driver name from your container instance\. If the driver was installed using another method, use Docker plugin discovery to retrieve the driver name\. For more information, see [Docker plugin discovery](https://docs.docker.com/engine/extend/plugin_api/#plugin-discovery)\. This parameter maps to `Driver` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--driver` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.  
`driverOpts`  
Type: String  
Required: No  
A map of Docker driver specific options to pass through\. This parameter maps to `DriverOpts` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--opt` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.  
`labels`  
Type: String  
Required: No  
Custom metadata to add to your Docker volume\. This parameter maps to `Labels` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--label` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.

`efsVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when using Amazon EFS volumes\.    
`fileSystemId`  
Type: String  
Required: Yes  
The Amazon EFS file system ID to use\.  
`rootDirectory`  
Type: String  
Required: No  
The directory within the Amazon EFS file system to mount as the root directory inside the host\. If this parameter is omitted, the root of the Amazon EFS volume will be used\. Specifying `/` will have the same effect as omitting this parameter\.  
If an EFS access point is specified in the `authorizationConfig`, the root directory parameter must either be omitted or set to `/` which will enforce the path set on the EFS access point\.  
`transitEncryption`  
Type: String  
Valid values: `ENABLED` \| `DISABLED`  
Required: No  
Whether or not to enable encryption for Amazon EFS data in transit between the Amazon ECS host and the Amazon EFS server\. Transit encryption must be enabled if Amazon EFS IAM authorization is used\. If this parameter is omitted, the default value of `DISABLED` is used\. For more information, see [Encrypting Data in Transit](https://docs.aws.amazon.com/efs/latest/ug/encryption-in-transit.html) in the *Amazon Elastic File System User Guide*\.  
`transitEncryptionPort`  
Type: Integer  
Required: No  
The port to use when sending encrypted data between the Amazon ECS host and the Amazon EFS server\. If you do not specify a transit encryption port, it will use the port selection strategy that the Amazon EFS mount helper uses\. For more information, see [EFS Mount Helper](https://docs.aws.amazon.com/efs/latest/ug/efs-mount-helper.html) in the *Amazon Elastic File System User Guide*\.  
`authorizationConfig`  
Type: Object  
Required: No  
The authorization configuration details for the Amazon EFS file system\.    
`accessPointId`  
Type: String  
Required: No  
The access point ID to use\. If an access point is specified, the root directory value in the `efsVolumeConfiguration` must either be omitted or set to `/` which will enforce the path set on the EFS access point\. If an access point is used, transit encryption must be enabled in the `EFSVolumeConfiguration`\. For more information, see [Working with Amazon EFS Access Points](https://docs.aws.amazon.com/efs/latest/ug/efs-access-points.html) in the *Amazon Elastic File System User Guide*\.  
`iam`  
Type: String  
Valid values: `ENABLED` \| `DISABLED`  
Required: No  
Whether or not to use the Amazon ECS task IAM role defined in a task definition when mounting the Amazon EFS file system\. If enabled, transit encryption must be enabled in the `EFSVolumeConfiguration`\. If this parameter is omitted, the default value of `DISABLED` is used\. For more information, see [IAM Roles for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)\.

`FSxWindowsFileServerVolumeConfiguration`  
Type: Object  
Required: Yes  
This parameter is specified when you are using the [FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) file system for task storage\.    
`fileSystemId`  
Type: String  
Required: Yes  
The FSx for Windows File Server file system ID to use\.  
`rootDirectory`  
Type: String  
Required: Yes  
The directory within the FSx for Windows File Server file system to mount as the root directory inside the host\.  
`authorizationConfig`    
`credentialsParameter`  
Type: String  
Required: Yes  
The authorization credential options\.  

**options:**
+ Amazon Resource Name \(ARN\) of an [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) secret\.
+ ARN of an [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html) parameter\.  
`domain`  
Type: String  
Required: Yes  
A fully qualified domain name hosted by an [AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html) Managed Microsoft AD \(Active Directory\) or self\-hosted EC2 AD\.

## Tags<a name="tags"></a>

When you register a task definition, you can optionally specify metadata tags that are applied to the task definition\. Tags help you categorize and organize your task definition\. Each tag consists of a key and an optional value\. You define both of them\. For more information, see [Tagging your Amazon ECS resources](ecs-using-tags.md)\.

**Important**  
Don't add personally identifiable information or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags aren't intended to be used for private or sensitive data\.

The following parameters are allowed in a tag object\.

`key`  
Type: string  
Required: no  
One part of a key\-value pair that make up a tag\. A key is a general label that acts like a category for more specific tag values\.

`value`  
Type: string  
Required: no  
The optional part of a key\-value pair that make up a tag\. A value acts as a descriptor within a tag category \(key\)\.

## Other task definition parameters<a name="other_task_definition_params"></a>

The following task definition parameters can be used when registering task definitions in the Amazon ECS console by using the **Configure via JSON** option\. For more information, see [Creating a task definition using the console](create-task-definition.md)\.

**Topics**
+ [Ephemeral storage](#task_definition_ephemeralStorage)
+ [IPC mode](#task_definition_ipcmode)
+ [PID mode](#task_definition_pidmode)

### Ephemeral storage<a name="task_definition_ephemeralStorage"></a>

`ephemeralStorage`  
Type: Object  
Required: No  
The amount of ephemeral storage \(in GB\) to allocate for the task\. This parameter is used to expand the total amount of ephemeral storage available, beyond the default amount, for tasks that are hosted on AWS Fargate\. For more information, see [Bind mounts](bind-mounts.md)\.  
This parameter is only supported for tasks that are hosted on AWS Fargate using platform version `1.4.0` or later \(Linux\)\. This isn't supported for Windows containers on Fargate\.

### IPC mode<a name="task_definition_ipcmode"></a>

`ipcMode`  
Type: String  
Required: No  
The IPC resource namespace to use for the containers in the task\. The valid values are `host`, `task`, or `none`\. If `host` is specified, then all the containers that are within the tasks that specified the `host` IPC mode on the same container instance share the same IPC resources with the host Amazon EC2 instance\. If `task` is specified, all the containers that are within the specified task share the same IPC resources\. If `none` is specified, then IPC resources within the containers of a task are private and not shared with other containers in a task or on the container instance\. If no value is specified, then the value for ipcMode is set to `shareable`\. For more information, see [IPC settings](https://docs.docker.com/engine/reference/run/#ipc-settings---ipc) in the *Docker run reference*\.  
If the `host` IPC mode is used, there's a heightened risk of undesired IPC namespace exposure\. For more information, see [Docker security](https://docs.docker.com/engine/security/security/)\.  
If you're setting namespaced kernel parameters that use `systemControls` for the containers in the task, the following applies to your IPC resource namespace\. For more information, see [System controls](#container_definition_systemcontrols)\.  
+ For tasks that use the `host` IPC mode, IPC namespace that's related `systemControls` aren't supported\.
+ For tasks that use the `task` IPC mode, `systemControls` that relate to the IPC namespace apply to all containers within a task\.

**Note**  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.

### PID mode<a name="task_definition_pidmode"></a>

`pidMode`  
Type: String  
Required: No  
The process namespace to use for the containers in the task\. The valid values are `host` or `task`\. If `host` is specified, all containers within the tasks that specified the `host` PID mode on the same container instance share the same process namespace with the host Amazon EC2 instance\. If `task` is specified, all containers within the specified task share the same process namespace\. If no value is specified, the default is a private namespace\. For more information, see [PID settings](https://docs.docker.com/engine/reference/run/#pid-settings---pid) in the *Docker run reference*\.  
If the `host` PID mode is used, there's a heightened risk of undesired process namespace exposure\. For more information, see [Docker security](https://docs.docker.com/engine/security/security/)\.

**Note**  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.
