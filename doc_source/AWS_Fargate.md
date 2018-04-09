# AWS Fargate on Amazon ECS<a name="AWS_Fargate"></a>

AWS Fargate is a technology that you can use with Amazon ECS to run [containers](https://aws.amazon.com/what-are-containers) without having to manage servers or clusters of EC2 instances\. With AWS Fargate, you no longer have to provision, configure, and scale clusters of virtual machines to run containers\. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing\.

When you run your tasks and services with the Fargate launch type, you package your application in containers, specify the CPU and memory requirements, define networking and IAM policies, and launch the application\.

This topic describes the different components of Fargate tasks and services, and calls out special considerations for using Fargate with Amazon ECS\.

AWS Fargate with Amazon ECS is currently only available in the following region:


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 

The following walkthroughs help you get started using AWS Fargate with Amazon ECS:
+ [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the AWS CLI](ECS_AWSCLI_Fargate.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the ECS CLI](ECS_CLI_tutorial_fargate.md)

## Task Definitions<a name="fargate-task-defs"></a>

Tasks that use the Fargate launch type do not support all of the task definition parameters that are available\. Some parameters are not supported at all, and others behave differently for Fargate tasks\.

The following task definition parameters are not valid in Fargate tasks:
+ `disableNetworking`
+ `dnsSearchDomain`
+ `dnsServers`
+ `dockerSecurityOptions`
+ `extraHosts`
+ `links`
+ `host` and `sourcePath`
+ `linuxParameters`
+ `placementConstraints`
+ `privileged`

To ensure that your task definition validates for use with the Fargate launch type, you can specify the following when you register the task definition: 
+ In the AWS Management Console, for the **Requires capabilities** field, specify `FARGATE`\.
+ In the AWS CLI, specify the `--requires-compatibilities` option\.
+ In the API, specify the `requiresCapabilities` flag\. 

### Network Mode<a name="fargate-tasks-networkmode"></a>

Fargate task definitions require that the network mode is set to `awsvpc`\. The `awsvpc` network mode provides each task with its own elastic network interface\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

A network configuration is also required when creating the service\. For more information, see [Task Networking](#fargate-tasks-services-networking)\.

### Task CPU and Memory<a name="fargate-tasks-size"></a>

Fargate task definitions require that you specify CPU and memory at the task level\. Although you can also specify CPU and memory at the container level for Fargate tasks, this is optional\. Most use cases are satisfied by only specifying these resources at the task level\. The table below shows the valid combinations of task\-level CPU and memory\.


| CPU value | Memory value | 
| --- | --- | 
| 256 \(\.25 vCPU\) | 0\.5 GB, 1 GB, 2 GB | 
| 512 \(\.5 vCPU\) | 1 GB, 2 GB, 3 GB, 4 GB | 
| 1024 \(1 vCPU\) | 2 GB, 3 GB, 4 GB, 5 GB, 6 GB, 7 GB, 8 GB | 
| 2048 \(2 vCPU\) | Between 4 GB and 16 GB in 1 GB increments | 
| 4096 \(4 vCPU\) | Between 8 GB and 30 GB in 1 GB increments | 

### Logging<a name="fargate-tasks-logging"></a>

Fargate task definitions only support the `awslogs` log driver for the log configuration\. This configures your Fargate tasks to send log information to Amazon CloudWatch Logs\. The following shows a snippet of a task definition where the awslogs log driver is configured:

```
"logConfiguration": { 
   "logDriver": "awslogs",
   "options": { 
      "awslogs-group" : "/ecs/fargate-task-definition",
      "awslogs-region": "us-east-1",
      "awslogs-stream-prefix": "ecs"
}
```

For more information about using the `awslogs` log driver in task definitions to send your container logs to CloudWatch Logs, see [Using the awslogs Log Driver](using_awslogs.md)\.

### Amazon ECS Task Execution IAM Role<a name="fargate-tasks-iam"></a>

There is an optional task execution IAM role that you can specify with Fargate to allow your Fargate tasks to make API calls to Amazon ECR\. The API calls pull container images as well as call CloudWatch to store container application logs\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

### Example Task Definition<a name="fargate-tasks-example"></a>

The following is an example task definition using the Fargate launch type that sets up a web server:

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
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.
**Note**  
The `host` and `sourcePath` parameters are not supported\.

For more information about Amazon ECS default service limits, see [Amazon ECS Service Limits](service_limits.md)\.

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
      }
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

After you have your Fargate task definition prepared, there are some considerations to make when creating your service\.

### Task Networking<a name="fargate-tasks-services-networking"></a>

Tasks using the Fargate launch type require the `awsvpc` network mode, which provides each task with an elastic network interface\. When you run a task or create a service with this network mode, you must specify one or more subnets to attach the network interface and one or more security groups to apply to the network interface\. 

Decide whether to provide a public IP address for the network interface\. For a Fargate task to pull container images, a public IP address needs to be assigned to the task's elastic network interface, with a route to the internet or a NAT gateway that can route requests to the internet For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

The following is an example of the networkConfiguration section for a Fargate service:

```
   "networkConfiguration": { 
      "awsvpcConfiguration": { 
         "assignPublicIp": "ENABLED",
         "securityGroups": [ "sg-12345678" ],
         "subnets": [ "subnet-12345678, subnet-87654321" ]
      }
   },
```

## Clusters<a name="fargate-clusters"></a>

Clusters can contain tasks using both the Fargate and EC2 launch types\. When viewing your clusters in the AWS Management Console, Fargate and EC2 task counts are displayed separately\.

For more information about Amazon ECS clusters, including a walkthrough for creating a cluster, see [Amazon ECS Clusters](ECS_clusters.md)\.