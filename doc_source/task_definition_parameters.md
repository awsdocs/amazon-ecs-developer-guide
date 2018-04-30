# Task Definition Parameters<a name="task_definition_parameters"></a>

Task definitions are split into separate parts: the task family, the IAM task role, the network mode, container definitions, volumes, task placement constraints, and launch types\. The family is the name of the task, and each family can have multiple revisions\. The IAM task role specifies the permissions that containers in the task should have\. The network mode determines how the networking is configured for your containers\. Container definitions specify which image to use, how much CPU and memory the container are allocated, and many more options\. Volumes allow you to share data between containers and even persist the data on the container instance when the containers are no longer running\. The task placement constraints customize how your tasks are placed within the infrastructure\. The launch type determines which infrastructure your tasks use\.

The family and container definitions are required in a task definition, while task role, network mode, volumes, task placement constraints, and launch type are optional\.

**Topics**
+ [Family](#family)
+ [Task Role](#task_role_arn)
+ [Task Execution Role](#execution_role_arn)
+ [Network Mode](#network_mode)
+ [Container Definitions](#container_definitions)
+ [Volumes](#volumes)
+ [Task Placement Constraints](#constraints)
+ [Launch Types](#requires_compatibilities)
+ [Task Size](#task_size)

## Family<a name="family"></a>

`family`  
Type: string  
Required: yes  
When you register a task definition, you give it a family, which is similar to a name for multiple versions of the task definition, specified with a revision number\. The first task definition that is registered into a particular family is given a revision of 1, and any task definitions registered after that are given a sequential revision number\.

## Task Role<a name="task_role_arn"></a>

`taskRoleArn`  
Type: string  
Required: no  
When you register a task definition, you can provide a task role for an IAM role that allows the containers in the task permission to call the AWS APIs that are specified in its associated policies on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.  
IAM roles for tasks on Windows require that the `-EnableTaskIAMRole` option is set when you launch the Amazon ECS\-optimized Windows AMI\. Your containers must also run some configuration code in order to take advantage of the feature\. For more information, see [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)\.

## Task Execution Role<a name="execution_role_arn"></a>

`executionRoleArn`  
Type: string  
Required: no  
When you register a task definition, you can provide a task execution role that allows the containers in the task to pull container images and publish container logs to CloudWatch on your behalf\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

## Network Mode<a name="network_mode"></a>

`networkMode`  
Type: string  
Required: no  
The Docker networking mode to use for the containers in the task\. The valid values are `none`, `bridge`, `awsvpc`, and `host`\. The default Docker network mode is `bridge`\. If using the Fargate launch type, the `awsvpc` network mode is required\. If using the EC2 launch type, any network mode can be used\. If the network mode is set to `none`, you can't specify port mappings in your container definitions, and the task's containers do not have external connectivity\. The `host` and `awsvpc` network modes offer the highest networking performance for containers because they use the Amazon EC2 network stack instead of the virtualized network stack provided by the `bridge` mode\.  
With the `host` and `awsvpc` network modes, exposed container ports are mapped directly to the corresponding host port \(for the `host` network mode\) or the attached elastic network interface port \(for the `awsvpc` network mode\), so you cannot take advantage of dynamic host port mappings\.   
If the network mode is `awsvpc`, the task is allocated an Elastic Network Interface, and you must specify a `NetworkConfiguration` when you create a service or run a task with the task definition\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.  
Currently, only the Amazon ECS\-optimized AMI, other Amazon Linux variants with the `ecs-init` package, or AWS Fargate infrastructure support the `awsvpc` network mode\. 
If the network mode is `host`, you can't run multiple instantiations of the same task on a single container instance when port mappings are used\.  
Docker for Windows uses different network modes than Docker for Linux\. When you register a task definition with Windows containers, you must not specify a network mode\. If you use the console to register a task definition with Windows containers, you must choose the `<default>` network mode object\. 

## Container Definitions<a name="container_definitions"></a>

When you register a task definition, you must specify a list of container definitions that are passed to the Docker daemon on a container instance\. The following parameters are allowed in a container definition\.

**Topics**
+ [Standard Container Definition Parameters](#standard_container_definition_params)
+ [Advanced Container Definition Parameters](#advanced_container_definition_params)

### Standard Container Definition Parameters<a name="standard_container_definition_params"></a>

The following task definition parameters are either required or used in most container definitions\.

`name`  
Type: string  
Required: yes  
The name of a container\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. If you are linking multiple containers together in a task definition, the `name` of one container can be entered in the `links` of another container to connect the containers\. This parameter maps to `name` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--name` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.

`image`  
Type: string  
Required: yes  
The image used to start a container\. This string is passed directly to the Docker daemon\. Images in the Docker Hub registry are available by default\. You can also specify other repositories with either `repository-url/image:tag` or `repository-url/image@digest`\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, underscores, colons, periods, forward slashes, and number signs are allowed\. This parameter maps to `Image` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `IMAGE` parameter of [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
+ When a new task starts, the Amazon ECS container agent pulls the latest version of the specified image and tag for the container to use\. However, subsequent updates to a repository image are not propagated to already running tasks\.
+ The Fargate launch type only supports images in Amazon ECR or public repositories in Docker Hub\.
+ Images in Amazon ECR repositories can be specified by using either the full `registry/repository:tag` or `registry/repository@digest` naming convention\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest` or `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app@sha256:94afd1f2e64d908bc90dbca0035a5b567EXAMPLE`
+ Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\)\.
+ Images in other online repositories are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu`\)\.

`memory`  
Type: integer  
Required: no  
The hard limit \(in MiB\) of memory to present to the container\. If your container attempts to exceed the memory specified here, the container is killed\. This parameter maps to `Memory` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--memory` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
If your containers will be part of a task using the Fargate launch type, this field is optional and the only requirement is that the total amount of memory reserved for all containers within a task be lower than the task `memory` value\.  
For containers that will be part of a task using the EC2 launch type, you must specify a non\-zero integer for one or both of `memory` or `memoryReservation` in container definitions\. If you specify both, `memory` must be greater than `memoryReservation`\. If you specify `memoryReservation`, then that value is subtracted from the available memory resources for the container instance on which the container is placed; otherwise, the value of `memory` is used\.  
The Docker daemon reserves a minimum of 4 MiB of memory for a container, so you should not specify fewer than 4 MiB of memory for your containers\.  
If you are trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, see [Container Instance Memory Management](memory-management.md)\.

`memoryReservation`  
Type: integer  
Required: no  
The soft limit \(in MiB\) of memory to reserve for the container\. When system memory is under contention, Docker attempts to keep the container memory to this soft limit; however, your container can consume more memory when it needs to, up to either the hard limit specified with the `memory` parameter \(if applicable\), or all of the available memory on the container instance, whichever comes first\. This parameter maps to `MemoryReservation` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--memory-reservation` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
You must specify a non\-zero integer for one or both of `memory` or `memoryReservation` in container definitions\. If you specify both, `memory` must be greater than `memoryReservation`\. If you specify `memoryReservation`, then that value is subtracted from the available memory resources for the container instance on which the container is placed; otherwise, the value of `memory` is used\.  
For example, if your container normally uses 128 MiB of memory, but occasionally bursts to 256 MiB of memory for short periods of time, you can set a `memoryReservation` of 128 MiB, and a `memory` hard limit of 300 MiB\. This configuration would allow the container to only reserve 128 MiB of memory from the remaining resources on the container instance, but also allow the container to consume more memory resources when needed\.  
The Docker daemon reserves a minimum of 4 MiB of memory for a container, so you should not specify fewer than 4 MiB of memory for your containers\.

`portMappings`  
Type: object array  
Required: no  
Port mappings allow containers to access ports on the host container instance to send or receive traffic\.  
For task definitions that use the `awsvpc` network mode, you should only specify the `containerPort`\. The `hostPort` can be left blank or it must be the same value as the `containerPort`\.  
Port mappings on Windows use the `NetNAT` gateway address rather than `localhost`\. There is no loopback for port mappings on Windows, so you cannot access a container's mapped port from the host itself\.   
This parameter maps to `PortBindings` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--publish` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. If the network mode of a task definition is set to `host`, then host ports must either be undefined or they must match the container port in the port mapping\.  
After a task reaches the `RUNNING` status, manual and automatic host and container port assignments are visible in the following locations:  
+ Console: The **Network Bindings** section of a container description for a selected task\.
+ AWS CLI: The `networkBindings` section of the describe\-tasks command output\.
+ API: The `DescribeTasks` response\.  
`containerPort`  
Type: integer  
Required: yes, when `portMappings` are used  
The port number on the container that is bound to the user\-specified or automatically assigned host port\.  
If using containers in a task with the Fargate launch type, exposed ports should be specified using `containerPort`\.  
If using containers in a task with the EC2 launch type and you specify a container port and not a host port, your container automatically receives a host port in the ephemeral port range \(for more information, see `hostPort`\)\. Port mappings that are automatically assigned in this way do not count toward the 100 reserved ports limit of a container instance\.  
`hostPort`  
Type: integer  
Required: no  
The port number on the container instance to reserve for your container\.  
If using containers in a task with the Fargate launch type, the `hostPort` can either be left blank or be the same value as `containerPort`\.  
If using containers in a task with the EC2 launch type, you can specify a non\-reserved host port for your container port mapping \(this is referred to as *static* host port mapping\), or you can omit the `hostPort` \(or set it to `0`\) while specifying a `containerPort` and your container automatically receives a port \(this is referred to as *dynamic* host port mapping\) in the ephemeral port range for your container instance operating system and Docker version\.  
The default ephemeral port range is `49153–65535`, and this range is used for Docker versions prior to 1\.6\.0\. For Docker version 1\.6\.0 and later, the Docker daemon tries to read the ephemeral port range from `/proc/sys/net/ipv4/ip_local_port_range` \(which is 32768–61000 on the latest Amazon ECS\-optimized AMI\); if this kernel parameter is unavailable, the default ephemeral port range is used\. Do not attempt to specify a host port in the ephemeral port range, as these are reserved for automatic assignment\. In general, ports below 32768 are outside of the ephemeral port range\.  
The default reserved ports are 22 for SSH, the Docker ports 2375 and 2376, and the Amazon ECS container agent port 51678\. Any host port that was previously user\-specified for a running task is also reserved while the task is running \(after a task stops, the host port is released\)\. The current reserved ports are displayed in the `remainingResources` of describe\-container\-instances output, and a container instance may have up to 100 reserved ports at a time, including the default reserved ports \(automatically assigned ports do not count toward the 100 reserved ports limit\)\.  
`protocol`  
Type: string  
Required: no  
The protocol used for the port mapping\. Valid values are `tcp` and `udp`\. The default is `tcp`\.  
UDP support is only available on container instances that were launched with version 1\.2\.0 of the Amazon ECS container agent \(such as the `amzn-ami-2015.03.c-amazon-ecs-optimized` AMI\) or later, or with container agents that have been updated to version 1\.3\.0 or later\. To update your container agent to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.
If you are specifying a host port, use the following syntax:  

```
"portMappings": [
    {
        "containerPort": integer,
        "hostPort": integer
    }
    ...
]
```
If you want an automatically assigned host port, use the following syntax:  

```
"portMappings": [
    {
        "containerPort": integer
    }
    ...
]
```

### Advanced Container Definition Parameters<a name="advanced_container_definition_params"></a>

The following advanced container definition parameters provide extended capabilities to the [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/) command that is used to launch containers on your Amazon ECS container instances\.

**Topics**
+ [Health Check](#container_definition_healthcheck)
+ [Environment](#container_definition_environment)
+ [Network Settings](#container_definition_network)
+ [Storage and Logging](#container_definition_storage)
+ [Security](#container_definition_security)
+ [Resource Limits](#container_definition_limits)
+ [Docker Labels](#container_definition_labels)
+ [Linux Parameters](#container_definition_linuxparameters)

#### Health Check<a name="container_definition_healthcheck"></a>

`healthCheck`  
The health check command and associated configuration parameters for the container\. This parameter maps to `HealthCheck` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `HEALTHCHECK` parameter of [docker run](https://docs.docker.com/engine/reference/run/)\.   
The Amazon ECS container agent only monitors and reports on the health checks specified in the task definition\. Amazon ECS does not monitor Docker health checks that are embedded in a container image and not specified in the container definition\. Health check parameters that are specified in a container definition override any Docker health checks that exist in the container image\.
Task health is reported by the `healthStatus` of the task, which is determined by the health of the essential containers in the task\. If all essential containers in the task are reporting as `HEALTHY`, then the task status also reports as `HEALTHY`\. If any essential containers in the task are reporting as `UNHEALTHY` or `UNKNOWN`, then the task status also reports as `UNHEALTHY` or `UNKNOWN`, accordingly\. If a service's task reports as unhealthy, it is removed from a service and replaced\.  
Container health checks require version 1\.17\.0 or greater of the Amazon ECS container agent\. For more information, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.
Container health checks are supported for Fargate tasks if using platform version v1\.1\.0 or later\. For more information, see [AWS Fargate Platform Versions](platform_versions.md)\.  
`command`  
A string array representing the command that the container runs to determine if it is healthy\. The string array must start with `CMD` to execute the command arguments directly, or `CMD-SHELL` to run the command with the container's default shell\.   
In the console, example input for a health check could be:  

```
CMD-SHELL, curl -f http://localhost/ || exit 1
```
Similarly, in the console JSON panel, the AWS CLI, or the APIs, example input for a health check could be:  

```
[ "CMD-SHELL", "curl -f http://localhost/ || exit 1" ]
```
An exit code of 0 indicates success, and a non\-zero exit code indicates failure\. For more information, see `HealthCheck` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/)\.  
`interval`  
The time period in seconds between each health check execution\. You may specify between 5 and 300 seconds\. The default value is 30 seconds\.  
`timeout`  
The time period in seconds to wait for a health check to succeed before it is considered a failure\. You may specify between 2 and 60 seconds\. The default value is 5 seconds\.  
`retries`  
The number of times to retry a failed health check before the container is considered unhealthy\. You may specify between 1 and 10 retries\. The default value is 3 retries\.  
`startPeriod`  
The optional grace period within which to provide containers time to bootstrap before failed health checks count towards the maximum number of retries\. You may specify between 0 and 300 seconds\. The `startPeriod` is disabled by default\.

#### Environment<a name="container_definition_environment"></a>

`cpu`  
Type: integer  
Required: no  
The number of `cpu` units to reserve for the container\. This parameter maps to `CpuShares` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--cpu-shares` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This field is optional for tasks using the Fargate launch type, and the only requirement is that the total amount of CPU reserved for all containers within a task be lower than the task\-level `cpu` value\.  
You can determine the number of CPU units that are available per Amazon EC2 instance type by multiplying the number of vCPUs listed for that instance type on the [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/) detail page by 1,024\.
Linux containers share unallocated CPU units with other containers on the container instance with the same ratio as their allocated amount\. For example, if you run a single\-container task on a single\-core instance type with 512 CPU units specified for that container, and that is the only task running on the container instance, that container could use the full 1,024 CPU unit share at any given time\. However, if you launched another copy of the same task on that container instance, each task would be guaranteed a minimum of 512 CPU units when needed, and each container could float to higher CPU usage if the other container was not using it, but if both tasks were 100% active all of the time, they would be limited to 512 CPU units\.  
On Linux container instances, the Docker daemon on the container instance uses the CPU value to calculate the relative CPU share ratios for running containers\. For more information, see [CPU share constraint](https://docs.docker.com/engine/reference/run/#cpu-share-constraint) in the Docker documentation\. The minimum valid CPU share value that the Linux kernel allows is 2\. However, the CPU parameter is not required, and you can use CPU values below 2 in your container definitions\. For CPU values below 2 \(including null\), the behavior varies based on your Amazon ECS container agent version:  
+ **Agent versions <= 1\.1\.0:** Null and zero CPU values are passed to Docker as 0, which Docker then converts to 1,024 CPU shares\. CPU values of 1 are passed to Docker as 1, which the Linux kernel converts to 2 CPU shares\.
+ **Agent versions >= 1\.2\.0:** Null, zero, and CPU values of 1 are passed to Docker as 2 CPU shares\.
On Windows container instances, the CPU limit is enforced as an absolute limit, or a quota\. Windows containers only have access to the specified amount of CPU that is described in the task definition\.

`essential`  
Type: Boolean  
Required: no  
If the `essential` parameter of a container is marked as `true`, and that container fails or stops for any reason, all other containers that are part of the task are stopped\. If the `essential` parameter of a container is marked as `false`, then its failure does not affect the rest of the containers in a task\. If this parameter is omitted, a container is assumed to be essential\.  
All tasks must have at least one essential container\. If you have an application that is composed of multiple containers, you should group containers that are used for a common purpose into components, and separate the different components into multiple task definitions\. For more information, see [Application Architecture](application_architecture.md)\.  

```
"essential": true|false
```

`entryPoint`  
Early versions of the Amazon ECS container agent do not properly handle `entryPoint` parameters\. If you have problems using `entryPoint`, update your container agent or enter your commands and arguments as `command` array items instead\.
Type: string array  
Required: no  
The entry point that is passed to the container\. This parameter maps to `Entrypoint` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--entrypoint` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. For more information about the Docker `ENTRYPOINT` parameter, go to [https://docs\.docker\.com/engine/reference/builder/\#entrypoint](https://docs.docker.com/engine/reference/builder/#entrypoint)\.   

```
"entryPoint": ["string", ...]
```

`command`  
Type: string array  
Required: no  
The command that is passed to the container\. This parameter maps to `Cmd` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `COMMAND` parameter to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. For more information about the Docker `CMD` parameter, go to [https://docs\.docker\.com/engine/reference/builder/\#cmd](https://docs.docker.com/engine/reference/builder/#cmd)\.   

```
"command": ["string", ...]
```

`workingDirectory`  
Type: string  
Required: no  
The working directory in which to run commands inside the container\. This parameter maps to `WorkingDir` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--workdir` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  

```
"workingDirectory": "string"
```

`environment`  
Type: object array  
Required: no  
The environment variables to pass to a container\. This parameter maps to `Env` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--env` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.  
`name`  
Type: string  
Required: yes, when `environment` is used  
The name of the environment variable\.  
`value`  
Type: string  
Required: yes, when `environment` is used  
The value of the environment variable\.

```
"environment" : [
    { "name" : "string", "value" : "string" },
    { "name" : "string", "value" : "string" }
]
```

#### Network Settings<a name="container_definition_network"></a>

`disableNetworking`  
Type: Boolean  
Required: no  
When this parameter is true, networking is disabled within the container\. This parameter maps to `NetworkDisabled` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/)\.  
This parameter is not supported for Windows containers\.

```
"disableNetworking": true|false
```

`links`  
Type: string array  
Required: no  
The `link` parameter allows containers to communicate with each other without the need for port mappings\. Only supported if the network mode of a task definition is set to `bridge`\. The `name:internalName` construct is analogous to `name:alias` in Docker links\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. For more information about linking Docker containers, go to [https://docs\.docker\.com/engine/userguide/networking/default\_network/dockerlinks/](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)\. This parameter maps to `Links` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--link` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.
Containers that are collocated on a single container instance may be able to communicate with each other without requiring links or host port mappings\. Network isolation is achieved on the container instance using security groups and VPC settings\.

```
"links": ["name:internalName", ...]
```

`hostname`  
Type: string  
Required: no  
The hostname to use for your container\. This parameter maps to `Hostname` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--hostname` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
The `hostname` parameter is not supported if using the `awsvpc` networkMode\.

```
"hostname": "string"
```

`dnsServers`  
Type: string array  
Required: no  
A list of DNS servers that are presented to the container\. This parameter maps to `Dns` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--dns` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

```
"dnsServers": ["string", ...]
```

`dnsSearchDomains`  
Type: string array  
Required: no  
A list of DNS search domains that are presented to the container\. This parameter maps to `DnsSearch` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--dns-search` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

```
"dnsSearchDomains": ["string", ...]
```

`extraHosts`  
Type: object array  
Required: no  
A list of hostnames and IP address mappings to append to the `/etc/hosts` file on the container\.   
This parameter maps to `ExtraHosts` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--add-host` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

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

#### Storage and Logging<a name="container_definition_storage"></a>

`readonlyRootFilesystem`  
Type: Boolean  
Required: no  
When this parameter is true, the container is given read\-only access to its root file system\. This parameter maps to `ReadonlyRootfs` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--read-only` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

```
"readonlyRootFilesystem": true|false
```

`mountPoints`  
Type: object array  
Required: no  
The mount points for data volumes in your container\.   
This parameter maps to `Volumes` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--volume` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers cannot mount directories on a different drive, and mount point cannot be across drives\.    
`sourceVolume`  
Type: string  
Required: yes, when `mountPoints` are used  
The name of the volume to mount\.  
`containerPath`  
Type: string  
Required: yes, when `mountPoints` are used  
The path on the container to mount the host volume at\.  
`readOnly`  
Type: Boolean  
Required: no  
If this value is `true`, the container has read\-only access to the volume\. If this value is `false`, then the container can write to the volume\. The default value is `false`\.

```
"mountPoints": [
                {
                  "sourceVolume": "string",
                  "containerPath": "string",
                  "readOnly": true|false
                }
              ]
```

`volumesFrom`  
Type: object array  
Required: no  
Data volumes to mount from another container\. This parameter maps to `VolumesFrom` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--volumes-from` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.    
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
Type: [LogConfiguration](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LogConfiguration.html) object  
Required: no  
The log configuration specification for the container\.   
If using the Fargate launch type, the only supported value is `awslogs`\. For more information on using the `awslogs` log driver in task definitions to send your container logs to CloudWatch Logs, see [Using the awslogs Log Driver](using_awslogs.md)\.  
This parameter maps to `LogConfig` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--log-driver` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\. By default, containers use the same logging driver that the Docker daemon uses; however the container may use a different logging driver than the Docker daemon by specifying a log driver with this parameter in the container definition\. To use a different logging driver for a container, the log system must be configured properly on the container instance \(or on a different log server for remote logging options\)\. For more information on the options for different supported log drivers, see [Configure logging drivers](https://docs.docker.com/engine/admin/logging/overview/) in the Docker documentation\.  
Amazon ECS currently supports a subset of the logging drivers available to the Docker daemon \(shown in the valid values below\)\. Additional log drivers may be available in future releases of the Amazon ECS container agent\.
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  
The Amazon ECS container agent running on a container instance must register the logging drivers available on that instance with the `ECS_AVAILABLE_LOGGING_DRIVERS` environment variable before containers placed on that instance can use these log configuration options\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

```
"logConfiguration": {
      "logDriver": "json-file"|"syslog"|"journald"|"gelf"|"fluentd"|"awslogs"|"splunk",
      "options": {"string": "string"
        ...}
```  
`logDriver`  
Type: string  
Valid values: `"json-file" | "syslog" | "journald" | "gelf" | "fluentd" | "awslogs" | "splunk"`  
Required: yes, when `logConfiguration` is used  
The log driver to use for the container\. The valid values listed earlier are log drivers that the Amazon ECS container agent can communicate with by default\.   
If using the Fargate launch type, the only supported value is `awslogs`\.  
If you have a custom driver that is not listed earlier that you would like to work with the Amazon ECS container agent, you can fork the Amazon ECS container agent project that is [available on GitHub](https://github.com/aws/amazon-ecs-agent) and customize it to work with that driver\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\.
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  
`options`  
Type: string to string map  
Required: no  
The configuration options to send to the log driver\.  
This parameter requires version 1\.19 of the Docker Remote API or greater on your container instance\.

#### Security<a name="container_definition_security"></a>

`privileged`  
Type: Boolean  
Required: no  
When this parameter is true, the container is given elevated privileges on the host container instance \(similar to the `root` user\)\.   
This parameter maps to `Privileged` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--privileged` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.

```
"privileged": true|false
```

`user`  
Type: string  
Required: no  
The user name to use inside the container\. This parameter maps to `User` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--user` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers\.

```
"user": "string"
```

`dockerSecurityOptions`  
Type: string array  
Required: no  
A list of strings to provide custom labels for SELinux and AppArmor multi\-level security systems\.   
This parameter maps to `SecurityOpt` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--security-opt` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
This parameter is not supported for Windows containers or tasks using the Fargate launch type\.

```
"dockerSecurityOptions": ["string", ...]
```
The Amazon ECS container agent running on a container instance must register with the `ECS_SELINUX_CAPABLE=true` or `ECS_APPARMOR_CAPABLE=true` environment variables before containers placed on that instance can use these security options\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

#### Resource Limits<a name="container_definition_limits"></a>

`ulimits`  
Type: object array  
Required: no  
A list of `ulimits` to set in the container\. This parameter maps to `Ulimits` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--ulimit` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.   
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

#### Docker Labels<a name="container_definition_labels"></a>

`dockerLabels`  
Type: string to string map  
Required: no  
A key/value map of labels to add to the container\. This parameter maps to `Labels` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--label` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.   
This parameter requires version 1\.18 of the Docker Remote API or greater on your container instance\.  

```
"dockerLabels": {"string": "string"
      ...}
```

#### Linux Parameters<a name="container_definition_linuxparameters"></a>

`linuxParameters`  
Type: [LinuxParameters](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LinuxParameters.html) object  
Required: no  
Linux\-specific options that are applied to the container, such as [KernelCapabilities](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_KernelCapabilities.html)\.  
This parameter is not supported for Windows containers\.

```
"linuxParameters": {
      "capabilities": {
        "add": ["string", ...],
        "drop": ["string", ...]
        }
      }
```  
`capabilities`  
Type: [KernelCapabilities](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_KernelCapabilities.html) object  
Required: no  
The Linux capabilities for the container that are added to or dropped from the default configuration provided by Docker\. For more information about the default capabilities and the non\-default available capabilities, see [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) in the *Docker run reference*\. For more detailed information about these Linux capabilities, see the [capabilities\(7\)](http://man7.org/linux/man-pages/man7/capabilities.7.html) Linux manual page\.  
If you are using tasks that use the Fargate launch type, `capabilities` is supported but the `add` parameter described below is not supported\.  
`add`  
Type: string array  
Valid values: `"ALL" | "AUDIT_CONTROL" | "AUDIT_WRITE" | "BLOCK_SUSPEND" | "CHOWN" | "DAC_OVERRIDE" | "DAC_READ_SEARCH" | "FOWNER" | "FSETID" | "IPC_LOCK" | "IPC_OWNER" | "KILL" | "LEASE" | "LINUX_IMMUTABLE" | "MAC_ADMIN" | "MAC_OVERRIDE" | "MKNOD" | "NET_ADMIN" | "NET_BIND_SERVICE" | "NET_BROADCAST" | "NET_RAW" | "SETFCAP" | "SETGID" | "SETPCAP" | "SETUID" | "SYS_ADMIN" | "SYS_BOOT" | "SYS_CHROOT" | "SYS_MODULE" | "SYS_NICE" | "SYS_PACCT" | "SYS_PTRACE" | "SYS_RAWIO" | "SYS_RESOURCE" | "SYS_TIME" | "SYS_TTY_CONFIG" | "SYSLOG" | "WAKE_ALARM"`  
Required: no  
The Linux capabilities for the container to add to the default configuration provided by Docker\. This parameter maps to `CapAdd` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--cap-add` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you are using tasks that use the Fargate launch type, the `add` parameter is not supported\.  
`drop`  
Type: string array  
Valid values: `"ALL" | "AUDIT_CONTROL" | "AUDIT_WRITE" | "BLOCK_SUSPEND" | "CHOWN" | "DAC_OVERRIDE" | "DAC_READ_SEARCH" | "FOWNER" | "FSETID" | "IPC_LOCK" | "IPC_OWNER" | "KILL" | "LEASE" | "LINUX_IMMUTABLE" | "MAC_ADMIN" | "MAC_OVERRIDE" | "MKNOD" | "NET_ADMIN" | "NET_BIND_SERVICE" | "NET_BROADCAST" | "NET_RAW" | "SETFCAP" | "SETGID" | "SETPCAP" | "SETUID" | "SYS_ADMIN" | "SYS_BOOT" | "SYS_CHROOT" | "SYS_MODULE" | "SYS_NICE" | "SYS_PACCT" | "SYS_PTRACE" | "SYS_RAWIO" | "SYS_RESOURCE" | "SYS_TIME" | "SYS_TTY_CONFIG" | "SYSLOG" | "WAKE_ALARM"`  
Required: no  
The Linux capabilities for the container to remove from the default configuration provided by Docker\. This parameter maps to `CapDrop` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--cap-drop` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
`devices`  
Any host devices to expose to the container\. This parameter maps to `Devices` in the [Create a container](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/#create-a-container) section of the [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.24/) and the `--device` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you are using tasks that use the Fargate launch type, the `devices` parameter is not supported\.
Type: Array of [Device](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Device.html) objects  
Required: No    
`hostPath`  
The path for the device on the host container instance\.  
Type: String  
Required: Yes  
`containerPath`  
The path inside the container at which to expose the host device\.  
Type: String  
Required: No  
`permissions`  
The explicit permissions to provide to the container for the device\. By default, the container can `read`, `write`, and `mknod` the device\.  
Type: Array of strings  
Valid Values: `read` \| `write` \| `mknod`  
`initProcessEnabled`  
Run an `init` process inside the container that forwards signals and reaps processes\. This parameter maps to the `--init` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
This parameter requires version 1\.25 of the Docker Remote API or greater on your container instance\.  
`sharedMemorySize`  
The value for the size \(in MiB\) of the `/dev/shm` volume\. This parameter maps to the `--shm-size` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you are using tasks that use the Fargate launch type, the `sharedMemorySize` parameter is not supported\.
Type: Integer  
`tmpfs`  
The container path, mount options, and size \(in MiB\) of the tmpfs mount\. This parameter maps to the `--tmpfs` option to [docker run](https://docs.docker.com/engine/reference/run/)\.  
If you are using tasks that use the Fargate launch type, the `tmpfs` parameter is not supported\.
Type: Array of [Tmpfs](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Tmpfs.html) objects  
Required: No    
`containerPath`  
The absolute file path where the tmpfs volume will be mounted\.  
Type: String  
Required: Yes  
`mountOptions`  
The list of tmpfs volume mount options\.  
Type: Array of strings  
Required: No  
Valid Values: `"defaults" | "ro" | "rw" | "suid" | "nosuid" | "dev" | "nodev" | "exec" | "noexec" | "sync" | "async" | "dirsync" | "remount" | "mand" | "nomand" | "atime" | "noatime" | "diratime" | "nodiratime" | "bind" | "rbind" | "unbindable" | "runbindable" | "private" | "rprivate" | "shared" | "rshared" | "slave" | "rslave" | "relatime" | "norelatime" | "strictatime" | "nostrictatime"`  
`size`  
The size \(in MiB\) of the tmpfs volume\.  
Type: Integer  
Required: Yes

## Volumes<a name="volumes"></a>

When you register a task definition, you can optionally specify a list of volumes that will be passed to the Docker daemon on a container instance and become available for other containers on the same container instance to access\. 

If you are using the Fargate launch type, the `host` and `sourcePath` parameters are not supported\.

For more information, see [Using Data Volumes in Tasks](using_data_volumes.md)\.

The following parameters are allowed in a container definition:

`name`  
Type: string  
Required: yes  
The name of the volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. This name is referenced in the `sourceVolume` parameter of container definition `mountPoints`\.

`host`  
Type: object  
Required: no  
The contents of the `host` parameter determine whether your data volume persists on the host container instance and where it is stored\. If the `host` parameter is empty, then the Docker daemon assigns a host path for your data volume, but the data is not guaranteed to persist after the containers associated with it stop running\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers cannot mount directories on a different drive, and mount point cannot be across drives\. For example, you can mount `C:\my\path:C:\my\path` and `D:\:D:\`, but not `D:\my\path:C:\my\path` or `D:\:C:\my\path`\.  
By default, Docker\-managed volumes are created in `/var/lib/docker/volumes/`\. You can change this default location by writing `OPTIONS="-g=/my/path/for/docker/volumes"` to `/etc/sysconfig/docker` on the container instance\.    
`sourcePath`  
Type: string  
Required: no  
The path on the host container instance that is presented to the container\. If this parameter is empty, then the Docker daemon assigns a host path for you\.  
If you are using the Fargate launch type, the `sourcePath` parameter is not supported\.  
If the `host` parameter contains a `sourcePath` file location, then the data volume persists at the specified location on the host container instance until you delete it manually\. If the `sourcePath` value does not exist on the host container instance, the Docker daemon creates it\. If the location does exist, the contents of the source path folder are exported\.

```
[
  {
    "name": "string",
    "host": {
      "sourcePath": "string"
    }
  }
]
```

## Task Placement Constraints<a name="constraints"></a>

When you register a task definition, you can provide task placement constraints that customize how Amazon ECS places tasks\.

If you are using the Fargate launch type, task placement contraints are not supported\. By default Fargate tasks are spread across availability zones\.

For tasks that use the EC2 launch type, you can use constraints to place tasks based on Availability Zone, instance type, or custom attributes\. For more information, see [Amazon ECS Task Placement Constraints](task-placement-constraints.md)\.

The following parameters are allowed in a container definition:

`expression`  
Type: string  
Required: no  
A cluster query language expression to apply to the constraint\. For more information, see [Cluster Query Language](cluster-query-language.md)\.

`type`  
Type: string  
Required: yes  
The type of constraint\. Use `memberOf` to restrict selection to a group of valid candidates\.

## Launch Types<a name="requires_compatibilities"></a>

When you register a task definition, you specify the launch type that you will be using for your task\. For more details about launch types, see [Amazon ECS Launch Types](launch_types.md)\.

The following parameter is allowed in a task definition:

`requiresCompatibilities`  
Type: string  
Required: no  
The launch type the task is using\. This will enable a check to ensure that all of the parameters used in the task definition meet the requirements of the launch type\.  
Valid values are `FARGATE` and `EC2`\. For more information about launch types, see [Amazon ECS Launch Types](launch_types.md)\.

## Task Size<a name="task_size"></a>

When you register a task definition, you can specify the total cpu and memory used for the task\. This is separate from the `cpu` and `memory` values at the container definition level\. If using the EC2 launch type, these fields are optional\. If using the Fargate launch type, these fields are required and there are specific values for both `cpu` and `memory` that are supported\.

**Note**  
Task\-level CPU and memory parameters are ignored for Windows containers\. We recommend specifying container\-level resources for Windows containers\.

The following parameter is allowed in a task definition:

`cpu`  
Type: string  
Required: no  
This parameter is not supported for Windows containers\.
The number of CPU units used by the task\. It can be expressed as an integer using CPU units, for example `1024`, or as a string using vCPUs, for example `1 vCPU` or `1 vcpu`, in a task definition but will be converted to an integer indicating the CPU units when the task definition is registered\.  
If using the EC2 launch type, this field is optional\. Supported values are between `128` CPU units \(`0.125` vCPUs\) and `10240` CPU units \(`10` vCPUs\)\.  
If using the Fargate launch type, this field is required and you must use one of the following values, which determines your range of supported values for the `memory` parameter:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)

`memory`  
Type: string  
Required: no  
This parameter is not supported for Windows containers\.
The amount of memory \(in MiB\) used by the task\. It can be expressed as an integer using MiB, for example `1024`, or as a string using GB, for example `1GB` or `1 GB`, in a task definition but will be converted to an integer indicating the MiB when the task definition is registered\.  
If using the EC2 launch type, this field is optional\.  
If using the Fargate launch type, this field is required and you must use one of the following values, which determines your range of supported values for the `cpu` parameter:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)