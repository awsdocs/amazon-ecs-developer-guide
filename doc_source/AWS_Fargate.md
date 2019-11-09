# Amazon ECS on AWS Fargate<a name="AWS_Fargate"></a>

AWS Fargate is a technology that you can use with Amazon ECS to run [containers](https://aws.amazon.com/what-are-containers) without having to manage servers or clusters of Amazon EC2 instances\. With AWS Fargate, you no longer have to provision, configure, or scale clusters of virtual machines to run containers\. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing\.

When you run your tasks and services with the Fargate launch type, you package your application in containers, specify the CPU and memory requirements, define networking and IAM policies, and launch the application\. Each Fargate task has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task\.

This topic describes the different components of Fargate tasks and services, and calls out special considerations for using Fargate with Amazon ECS\.

AWS Fargate with Amazon ECS is currently only available in the following Regions:


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US East \(Ohio\) | us\-east\-2 | 
| US West \(N\. California\) | us\-west\-1 | 
| US West \(Oregon\) | us\-west\-2 | 
| Asia Pacific \(Hong Kong\) | ap\-east\-1 | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | 
| Canada \(Central\) | ca\-central\-1 | 
| EU \(Frankfurt\) | eu\-central\-1 | 
| EU \(Ireland\) | eu\-west\-1 | 
| EU \(London\) | eu\-west\-2 | 
| EU \(Paris\) | eu\-west\-3 | 
| EU \(Stockholm\) | eu\-north\-1 | 
| Middle East \(Bahrain\) | me\-south\-1 | 
| AWS GovCloud \(US\-East\) | us\-gov\-east\-1 | 
| AWS GovCloud \(US\) | us\-gov\-west\-1 | 
| China \(Beijing\) | cn\-north\-1 | 
| China \(Ningxia\) | cn\-northwest\-1 | 

The following walkthroughs help you get started using AWS Fargate with Amazon ECS:
+ [Getting Started with Amazon ECS](ECS_GetStarted.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the AWS CLI](ECS_AWSCLI_Fargate.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the Amazon ECS CLI](ecs-cli-tutorial-fargate.md)

## Task Definitions<a name="fargate-task-defs"></a>

Tasks that use the Fargate launch type do not support all of the task definition parameters that are available\. Some parameters are not supported at all, and others behave differently for Fargate tasks\.

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
+ `linuxParameters` – When specifying Linux\-specific options that are applied to the container, for `capabilities` the `add` parameter is not supported\. The `devices`, `sharedMemorySize`, and `tmpfs` parameters are not supported\. For more information, see [Linux Parameters](task_definition_parameters.md#container_definition_linuxparameters)\.
+ `volumes` – Fargate tasks only support bind mount host volumes, so the `dockerVolumeConfiguration` parameter is not supported\. For more information, see [Volumes](task_definition_parameters.md#volumes)\.

To ensure that your task definition validates for use with the Fargate launch type, you can specify the following when you register the task definition: 
+ In the AWS Management Console, for the **Requires Compatibilities** field, specify `FARGATE`\.
+ In the AWS CLI, specify the `--requires-compatibilities` option\.
+ In the Amazon ECS API, specify the `requiresCompatibilities` flag\.

### Network Mode<a name="fargate-tasks-networkmode"></a>

Fargate task definitions require that the network mode is set to `awsvpc`\. The `awsvpc` network mode provides each task with its own elastic network interface\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

A network configuration is also required when creating a service or manually running tasks\. For more information, see [Task Networking](#fargate-tasks-services-networking)\.

### Task CPU and Memory<a name="fargate-tasks-size"></a>

Fargate task definitions require that you specify CPU and memory at the task level\. Although you can also specify CPU and memory at the container level for Fargate tasks, this is optional\. Most use cases are satisfied by only specifying these resources at the task level\. The table below shows the valid combinations of task\-level CPU and memory\.


| CPU value | Memory value | 
| --- | --- | 
| 256 \(\.25 vCPU\) | 0\.5 GB, 1 GB, 2 GB | 
| 512 \(\.5 vCPU\) | 1 GB, 2 GB, 3 GB, 4 GB | 
| 1024 \(1 vCPU\) | 2 GB, 3 GB, 4 GB, 5 GB, 6 GB, 7 GB, 8 GB | 
| 2048 \(2 vCPU\) | Between 4 GB and 16 GB in 1\-GB increments | 
| 4096 \(4 vCPU\) | Between 8 GB and 30 GB in 1\-GB increments | 

### Logging<a name="fargate-tasks-logging"></a>

Fargate task definitions only support the `awslogs` and `splunk` log drivers for the log configuration\. 

The `awslogs` log driver configures your Fargate tasks to send log information to Amazon CloudWatch Logs\. The following shows a snippet of a task definition where the awslogs log driver is configured:

```
"logConfiguration": { 
   "logDriver": "awslogs",
   "options": { 
      "awslogs-group" : "/ecs/fargate-task-definition",
      "awslogs-region": "us-east-1",
      "awslogs-stream-prefix": "ecs"
}
```

For more information about using the `awslogs` log driver in a task definition to send your container logs to CloudWatch Logs, see [Using the awslogs Log Driver](using_awslogs.md)\.

For more information about using the `splunk` log driver in a task definition, see [Example: `splunk` Log Driver](example_task_definitions.md#example_task_definition-splunk)\.

### Amazon ECS Task Execution IAM Role<a name="fargate-tasks-iam"></a>

There is an optional task execution IAM role that you can specify with Fargate to allow your Fargate tasks to make API calls to Amazon ECR\. The API calls pull container images as well as calling CloudWatch to store container application logs\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

### Example Task Definition<a name="fargate-tasks-example"></a>

The following is an example task definition that sets up a web server using the Fargate launch type:

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
   "memory": "512",
   "networkMode": "awsvpc",
   "requiresCompatibilities": [ 
       "FARGATE" 
    ]
}
```

### Task Storage<a name="fargate-tasks-storage"></a>

When provisioned, each Fargate task receives the following storage\. Task storage is ephemeral\. After a Fargate task stops, the storage is deleted\.
+ 10 GB of Docker layer storage
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints`, and `volumesFrom` parameters in the task definition\.
**Note**  
The `host` and `sourcePath` parameters are not supported\.

For more information about Amazon ECS default service quotas, see [Amazon ECS Service Quotas](service-quotas.md)\.

The following shows a snippet of a task definition where two containers are sharing a single volume:

```
{
   "containerDefinitions": [ 
      { 
         "image": "my-repo/database",
         "mountPoints": [ 
            { 
               "containerPath": "/var/scratch",
               "sourceVolume": "database_scratch"
            }
         ],
         "name": "database1",
      },
      { 
         "image": "my-repo/database",
         "mountPoints": [ 
            { 
               "containerPath": "/var/scratch",
               "sourceVolume": "database_scratch"
            }
         ],
         "name": "database2",
      }
   ],
   "volumes": [ 
      { 
         "name": "database_scratch"
      }
   ]
}
```

## Tasks and Services<a name="fargate-tasks-services"></a>

After you have your Fargate task definition prepared, there are some decisions to make when creating your service\.

### Task Networking<a name="fargate-tasks-services-networking"></a>

Tasks using the Fargate launch type require the `awsvpc` network mode, which provides each task with an elastic network interface\. When you run a task or create a service with this network mode, you must specify one or more subnets to attach the network interface and one or more security groups to apply to the network interface\. 

If you are using public subnets, decide whether to provide a public IP address for the network interface\. For a Fargate task in a public subnet to pull container images, a public IP address needs to be assigned to the task's elastic network interface, with a route to the internet or a NAT gateway that can route requests to the internet\. For a Fargate task in a private subnet to pull container images, the private subnet requires a NAT gateway be attached to route requests to the internet\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

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

Services with tasks that use the `awsvpc` network mode \(for example, those with the Fargate launch type\) only support Application Load Balancers and Network Load Balancers\. Classic Load Balancers are not supported\. Also, when you create any target groups for these services, you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\. For more information, see [Service Load Balancing](service-load-balancing.md)\.

## Private Registry Authentication<a name="fargate-private-auth-reg"></a>

Fargate tasks can authenticate with private image registries, including Docker Hub, using basic authentication\. When you enable private registry authentication, you can use private Docker images in your task definitions\.

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

For more information, see [Private Registry Authentication for Tasks](private-auth.md)\.

## Clusters<a name="fargate-clusters"></a>

Clusters can contain tasks using both the Fargate and EC2 launch types\. When viewing your clusters in the AWS Management Console, Fargate and EC2 task counts are displayed separately\.

For more information about Amazon ECS clusters, including a walkthrough for creating a cluster, see [Amazon ECS Clusters](ECS_clusters.md)\.

## Fargate Task Retirement<a name="fargate-task-retirement"></a>

A Fargate task is scheduled to be retired when AWS detects the irreparable failure of the underlying hardware hosting the task or if a security issue needs to be patched\. Most security patches are handled transparently without requiring any action on your part or having to restart your tasks\. But for certain issues, we may require that the task be restarted\. 

When a task reaches its scheduled retirement date, it is stopped or terminated by AWS\. If the task is part of a service, then the task is automatically stopped and the service scheduler starts a new one to replace it\. If you are using standalone tasks, then you receive notification of the task retirement\. For more information, see [Task Retirement](task-retirement.md)\.

## Fargate Savings Plans<a name="fargate-savings-plans"></a>

Savings Plans are a pricing model that offer significant savings on AWS usage\. You commit to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years, and receive a lower price for that usage\. For more information, see the [Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)\.

To create a Savings Plan for your Fargate usage, use the **Compute Savings Plans** type\. For more information, see [Savings Plans and Amazon ECS](savings-plans.md)\.