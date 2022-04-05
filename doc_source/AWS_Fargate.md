# Amazon ECS on AWS Fargate<a name="AWS_Fargate"></a>

AWS Fargate is a technology that you can use with Amazon ECS to run [containers](https://aws.amazon.com/what-are-containers) without having to manage servers or clusters of Amazon EC2 instances\. With AWS Fargate, you no longer have to provision, configure, or scale clusters of virtual machines to run containers\. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing\.

When you run your tasks and services with the Fargate launch type, you package your application in containers, specify the CPU and memory requirements, define networking and IAM policies, and launch the application\. Each Fargate task has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task\.

Fargate offers platform versions for Amazon Linux 2 and Microsoft Windows 2019 Server Full and Core editions\. Unless otherwise specified, the information on this page applies to all Fargate platforms\.

This topic describes the different components of Fargate tasks and services, and calls out special considerations for using Fargate with Amazon ECS\.

For information about the Regions that support Linux containers on Fargate, see [Supported Regions for Linux containers on AWS Fargate](AWS_Fargate-Regions.md#linux-regions)\.

For information about the Regions that support Windows containers on Fargate, see [Supported Regions for Windows containers on AWS Fargate](AWS_Fargate-Regions.md#windows-regions)\.

## Task definitions<a name="fargate-task-defs"></a>

Amazon ECS tasks on AWS Fargate do not support all of the task definition parameters that are available\. Some parameters are not supported at all, and others behave differently for Fargate tasks\.

The following task definition parameters are not valid in Fargate tasks:
+ `disableNetworking`
+ `dnsSearchDomains`
+ `dnsServers`
+ `dockerSecurityOptions`
+ `extraHosts`
+ `gpu`
+ `ipcMode`
+ `links`
+ `pidMode`
+ `placementConstraints`
+ `privileged`
+ `systemControls`

The following task definition parameters are valid in Fargate tasks, but have limitations that should be noted:
+ `linuxParameters` – When specifying Linux\-specific options that are applied to the container, for `capabilities` the `add` parameter is not supported\. The `devices`, `sharedMemorySize`, and `tmpfs` parameters are not supported\. For more information, see [Linux parameters](task_definition_parameters.md#container_definition_linuxparameters)\.
+ `volumes` – Fargate tasks only support bind mount host volumes, so the `dockerVolumeConfiguration` parameter is not supported\. For more information, see [Volumes](task_definition_parameters.md#volumes)\.
+ `cpu` \- For Windows containers on AWS Fargate, the value cannot be less than 1 vCPU\.

To ensure that your task definition validates for use with Fargate, you can specify the following when you register the task definition: 
+ In the AWS Management Console, for the **Requires Compatibilities** field, specify `FARGATE`\.
+ In the AWS CLI, specify the `--requires-compatibilities` option\.
+ In the Amazon ECS API, specify the `requiresCompatibilities` flag\.

### Network mode<a name="fargate-tasks-networkmode"></a>

Amazon ECS task definitions for AWS Fargate require that the network mode is set to `awsvpc`\. The `awsvpc` network mode provides each task with its own elastic network interface\. For more information, see [AWS Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

A network configuration is also required when creating a service or manually running tasks\. For more information, see [AWS Fargate task networking ](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html)in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

### Task Operating Systems<a name="fargate-task-os"></a>

When you configure a task and container definition for AWS Fargate, you must specify the Operating System that the container runs\. The following Operating Systems are supported for AWS Fargate:
+ Amazon Linux 2
+ Windows Server 2019 Full
+ Windows Server 2019 Core

### Task CPU architecture<a name="fargate-task-cpu-architecture"></a>

There are 2 architectures available for the Amazon ECS task definition, ARM and X86\_64\.

When you run Windows containers on AWS Fargate, you must have the X86\_64 CPU architecture\.

When you run Linux containers on AWS Fargate, you can use the X86\_64 CPU architecture, or the ARM64 architecture for your ARM\-based applications\. For more information, see [Working with 64\-bit ARM workloads on Amazon ECS](ecs-arm64.md)\.

### Task CPU and memory<a name="fargate-tasks-size"></a>

Amazon ECS task definitions for AWS Fargate require that you specify CPU and memory at the task level\. Although you can also specify CPU and memory at the container level for Fargate tasks, this is optional\. Most use cases are satisfied by only specifying these resources at the task level\. The table below shows the valid combinations of task\-level CPU and memory\.


| CPU value | Memory value | Operating systems supported for AWS Fargate  | 
| --- | --- | --- | 
| 256 \(\.25 vCPU\) | 512 MB, 1 GB, 2 GB | Linux | 
| 512 \(\.5 vCPU\) | 1 GB, 2 GB, 3 GB, 4 GB | Linux | 
| 1024 \(1 vCPU\) | 2 GB, 3 GB, 4 GB, 5 GB, 6 GB, 7 GB, 8 GB | Linux, Windows | 
| 2048 \(2 vCPU\) | Between 4 GB and 16 GB in 1 GB increments | Linux, Windows | 
| 4096 \(4 vCPU\) | Between 8 GB and 30 GB in 1 GB increments | Linux, Windows | 

### Task resource limits<a name="fargate-resource-limits"></a>

Amazon ECS task definitions for Linux containers on AWS Fargate support the `ulimits` parameter to define the resource limits to set for a container\.

Amazon ECS task definitions for Windows on AWS Fargate do not support the `ulimits` parameter to define the resource limits to set for a container\.

Amazon ECS tasks hosted on Fargate use the default resource limit values set by the operating system with the exception of the `nofile` resource limit parameter which Fargate overrides\. The `nofile` resource limit sets a restriction on the number of open files that a container can use\. The default `nofile` soft limit is `1024` and hard limit is `4096`\.

The following is an example task definition snippet that shows how to define a custom `nofile` limit that has been doubled:

```
"ulimits": [
    {
       "name": "nofile",
       "softLimit": 2048,
       "hardLimit": 8192
    }
]
```

For more information on the other resource limits that can be adjusted, see [Resource limits](task_definition_parameters.md#container_definition_limits)\.

### Logging<a name="fargate-tasks-logging"></a>

Amazon ECS task definitions for AWS Fargate support the `awslogs`, `splunk`, and `firelens` log drivers for the log configuration\.

The `awslogs` log driver configures your Fargate tasks to send log information to Amazon CloudWatch Logs\. The following shows a snippet of a task definition where the `awslogs` log driver is configured:

```
"logConfiguration": { 
   "logDriver": "awslogs",
   "options": { 
      "awslogs-group" : "/ecs/fargate-task-definition",
      "awslogs-region": "us-east-1",
      "awslogs-stream-prefix": "ecs"
   }
}
```

For more information about using the `awslogs` log driver in a task definition to send your container logs to CloudWatch Logs, see [Using the awslogs log driver](using_awslogs.md)\.

For more information about the `firelens` log driver in a task definition, see [Custom log routing](using_firelens.md)\.

For more information about using the `splunk` log driver in a task definition, see [Example: `splunk` log driver](example_task_definitions.md#example_task_definition-splunk)\.

### Amazon ECS task execution IAM role<a name="fargate-tasks-iam"></a>

There is an optional task execution IAM role that you can specify with Fargate to allow your Fargate tasks to make API calls to Amazon ECR\. The API calls pull container images as well as calling CloudWatch to store container application logs\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

### Example Amazon Linux 2 task definition<a name="fargate-tasks-example"></a>

The following is an example task definition that sets up a web server using the Fargate launch type with an Amazon Linux 2 operating system:

```
{
   "containerDefinitions": [ 
      { 
         "command": [
            "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
         ],
         "entryPoint": [
            "sh",
            "-c"
         ],
         "essential": true,
         "image": "httpd:2.4",
         "logConfiguration": { 
            "logDriver": "awslogs",
            "options": { 
               "awslogs-group" : "/ecs/fargate-task-definition",
               "awslogs-region": "us-east-1",
               "awslogs-stream-prefix": "ecs"
            }
         },
         "name": "sample-fargate-app",
         "portMappings": [ 
            { 
               "containerPort": 80,
               "hostPort": 80,
               "protocol": "tcp"
            }
         ]
      }
   ],
   "cpu": "256",
   "executionRoleArn": "arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
   "family": "fargate-task-definition",
   "runtimePlatform": {
        "operatingSystemFamily": "LINUX"
    },
   "memory": "512",
   "networkMode": "awsvpc",
   "requiresCompatibilities": [ 
       "FARGATE" 
    ]
}
```

### Example Windows task definition<a name="fargate-windows-tasks-example"></a>

The following is an example task definition that sets up a web server using the Fargate launch type with a Windows 2019 Server operating system\. 

```
{
    "containerDefinitions": [
        {
            "command": [
                "New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"
            ],
            "entryPoint": [
                "powershell",
                "-Command"
            ],
            "essential": true,
            "cpu": 2048,
            "memory": 4096,      
            "image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "/ecs/fargate-windows-task-definition",
                    "awslogs-region": "us-east-1",
                    "awslogs-stream-prefix": "ecs"
                }
            },
            "name": "sample_windows_app",
            "portMappings": [
                {
                    "hostPort": 80,
                    "containerPort": 80,
                    "protocol": "tcp"
                }
            ]
        }
    ],
    "memory": "4096",
    "cpu": "2048",
    "networkMode": "awsvpc",
    "family": "windows-simple-iis-2019-core",
    "executionRoleArn": "arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
    "runtimePlatform": {
        "operatingSystemFamily": "WINDOWS_SERVER_2019_CORE"
    },
    "requiresCompatibilities": [
        "FARGATE"
    ]
}
```

### Task storage<a name="fargate-tasks-storage"></a>

For Amazon ECS tasks hosted on Fargate, the following storage types are supported:
+ Amazon EFS volumes for persistent storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.
+ Bind mounts for ephemeral storage\. For more information, see [Bind mounts](bind-mounts.md)\.

## Tasks and services<a name="fargate-tasks-services"></a>

After you have your Amazon ECS task definitions for AWS Fargate prepared, there are some decisions to make when creating your service\.

### Task networking<a name="fargate-tasks-services-networking"></a>

Amazon ECS tasks for AWS Fargate require the `awsvpc` network mode, which provides each task with an elastic network interface\. When you run a task or create a service with this network mode, you must specify one or more subnets to attach the network interface and one or more security groups to apply to the network interface\. 

If you are using public subnets, decide whether to provide a public IP address for the network interface\. For a Fargate task in a public subnet to pull container images, a public IP address needs to be assigned to the task's elastic network interface, with a route to the internet or a NAT gateway that can route requests to the internet\. For a Fargate task in a private subnet to pull container images, you need a NAT gateway in the subnet to route requests to the internet\. When you host your container images in Amazon ECR, you can configure Amazon ECR to use an interface VPC endpoint\. In this case, the task's private IPv4 address is used for the image pull\. For more information about Amazon ECR interface endpoints, see [Amazon ECR interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html) in the *Amazon Elastic Container Registry User Guide*\.

The following is an example of the networkConfiguration section for a Fargate service:

```
"networkConfiguration": { 
   "awsvpcConfiguration": { 
      "assignPublicIp": "ENABLED",
      "securityGroups": [ "sg-12345678" ],
      "subnets": [ "subnet-12345678" ]
   }
}
```

### Service load balancing<a name="fargate-tasks-services-load-balancing"></a>

Your Amazon ECS service on AWS Fargate can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service\.

Amazon ECS services on AWS Fargate support the Application Load Balancer and Network Load Balancer load balancer types\. Application Load Balancers are used to route HTTP/HTTPS \(or layer 7\) traffic\. Network Load Balancers are used to route TCP or UDP \(or layer 4\) traffic\. For more information, see [Load balancer types](load-balancer-types.md)\.

When you create a target group for these services, you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\. For more information, see [Service load balancing](service-load-balancing.md)\.

Using a Network Load Balancer to route UDP traffic to your Amazon ECS on AWS Fargate tasks is only supported when using platform version 1\.4 or and for tasks launched in the following Regions:
+ US East \(N\. Virginia\) \- `us-east-1`
+ US West \(Oregon\) \- `us-west-2`
+ EU \(Ireland\) \- `eu-west-1`
+ Asia Pacific \(Tokyo\) \- `ap-northeast-1`

## Private registry authentication<a name="fargate-private-auth-reg"></a>

Amazon ECS tasks for AWS Fargate can authenticate with private image registries, including Docker Hub, using basic authentication\. When you enable private registry authentication, you can use private Docker images in your task definitions\.

To use private registry authentication, you create a secret with AWS Secrets Manager containing the credentials for your private registry\. Then, within your container definition, you specify `repositoryCredentials` with the full ARN of the secret that you created\. The following snippet of a task definition shows the required parameters:

```
"containerDefinitions": [
   {
      "image": "private-repo/private-image",
      "repositoryCredentials": {
         "credentialsParameter: "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name"
      }
   }
]
```

For more information, see [Private registry authentication for tasks](private-auth.md)\.

## Clusters<a name="fargate-clusters"></a>

Clusters may contain tasks using both the Fargate and EC2 launch types\. When viewing your clusters in the AWS Management Console, Fargate and EC2 task counts are displayed separately\.

For more information about Amazon ECS clusters, including a walkthrough for creating a cluster, see [Amazon ECS clusters](clusters.md)\.

## Fargate Spot<a name="fargate-spot"></a>

Amazon ECS capacity providers enable you to use both AWS Fargate and Fargate Spot capacity with your Amazon ECS tasks\.

Windows containers on AWS Fargate cannot use the Fargate Spot capacity provider\.

With Fargate Spot you can run interruption tolerant Amazon ECS tasks at a discounted rate compared to the AWS Fargate price\. Fargate Spot runs tasks on spare compute capacity\. When AWS needs the capacity back, your tasks will be interrupted with a two\-minute warning\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.

## Usage metrics<a name="fargate-usage-metrics"></a>

You can use CloudWatch usage metrics to provide visibility into your accounts usage of resources\. Use these metrics to visualize your current service usage on CloudWatch graphs and dashboards\.

AWS Fargate usage metrics correspond to AWS service quotas\. You can configure alarms that alert you when your usage approaches a service quota\. For more information about AWS Fargate service quotas, see [AWS Fargate service quotas](service-quotas.md#service-quotas-fargate)\.

For more information about AWS Fargate usage metrics, see [AWS Fargate usage metrics](https://docs.aws.amazon.com/AmazonECS/latest/userguide/monitoring-fargate-usage.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

## Task maintenance<a name="fargate-task-retirement"></a>

When AWS determines that a security or infrastructure update is needed for an Amazon ECS task hosted on AWS Fargate, the tasks need to be stopped and new tasks launched to replace them\. For more information, see [Task maintenance](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task-maintenance.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

The following table describes these scenarios\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)

## Savings plans<a name="fargate-savings-plans"></a>

Savings Plans are a pricing model that offer significant savings on AWS usage\. You commit to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years, and receive a lower price for that usage\. For more information, see the [Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)\.

To create a Savings Plan for your AWS Fargate usage, use the **Compute Savings Plans** type\. To get started, see [Getting started with Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/get-started.html) in the *Savings Plans User Guide*\.

## Windows containers on AWS Fargate considerations<a name="windows-considerations"></a>

Windows containers on AWS Fargate supports the following operating systems:
+ Windows Server 2019 Full
+ Windows Server 2019 Core

AWS handles the operating system license management, so you do not need any additional Microsoft licenses\.

Windows containers on AWS Fargate supports the awslogs driver\. For more information, see [Using the awslogs log driver](using_awslogs.md)\.

Your tasks can run either Linux containers or Windows containers\. If you need run both container types, you must create separate tasks\.

The following features are not supported on Windows containers on Fargate:
+ Group managed service accounts \(gMSA\)
+ Amazon FSx
+ ENI trunking
+ App Mesh service and proxy integration for tasks
+ Firelens log router integration for tasks
+ Configurable ephemeral storage
+ EFS volumes
+ The Fargate Spot capacity provider
+ Image volumes

  The Dockerfile `volume` option is ignored\. Instead, use bind mounts in your task definition\. For more information, see [Bind mounts](bind-mounts.md)\. 

## Getting started walkthroughs<a name="welcome-tutorials"></a>

The following walkthroughs help you get started using AWS Fargate with Amazon ECS:
+ [Getting started with the classic console using Linux containers on AWS Fargate](getting-started-fargate.md)
+ [Getting started with the classic Amazon ECS console using Windows containers on AWS Fargate](Windows_fargate-getting_started.md)
+ [Tutorial: Creating a cluster with a Fargate Linux task using the AWS CLI](ECS_AWSCLI_Fargate.md)
+ [Tutorial: Creating a cluster with a Fargate Windows task using the AWS CLI](ECS_AWSCLI_Fargate_windows.md)
+ [Tutorial: Creating a cluster with a Fargate task using the Amazon ECS CLI](ecs-cli-tutorial-fargate.md)