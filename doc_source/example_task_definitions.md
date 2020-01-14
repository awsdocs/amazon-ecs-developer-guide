# Example Task Definitions<a name="example_task_definitions"></a>

This section provides some task definition examples that you can use to start creating your own task definitions\. For more information, see [Task Definition Parameters](task_definition_parameters.md) and [Creating a Task Definition](create-task-definition.md)\.

For additional task definition examples, see [AWS Sample Task Definitions](https://github.com/aws-samples/aws-containers-task-definitions) on GitHub\.

**Topics**
+ [Example: Webserver](#example_task_definition-webserver)
+ [Example: WordPress and MySQL](#example_task_definition-wordpress)
+ [Example: `awslogs` Log Driver](#example_task_definition-awslogs)
+ [Example: `splunk` Log Driver](#example_task_definition-splunk)
+ [Example: `fluentd` Log Driver](#example_task_definition-fluentd)
+ [Example: `gelf` Log Driver](#example_task_definition-gelf)
+ [Example: Amazon ECR Image and Task Definition IAM Role](#example_task_definition-iam)
+ [Example: Entrypoint with Command](#example_task_definition-ping)
+ [Example: Container Dependency](#example_task_definition-containerdependency)

## Example: Webserver<a name="example_task_definition-webserver"></a>

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

## Example: WordPress and MySQL<a name="example_task_definition-wordpress"></a>

The following example specifies a WordPress container and a MySQL container that are linked together\. This WordPress container exposes the container port 80 on the host port 80\. The security group on the container instance would need to open port 80 in order for this WordPress installation to be accessible from a web browser\.

For more information about the WordPress container, see the official WordPress Docker Hub repository at [https://registry\.hub\.docker\.com/\_/wordpress/](https://registry.hub.docker.com/_/wordpress/)\. For more information about the MySQL container, go to the official MySQL Docker Hub repository at [https://registry\.hub\.docker\.com/\_/mysql/](https://registry.hub.docker.com/_/mysql/)\.

```
{
  "containerDefinitions": [
    {
      "name": "wordpress",
      "links": [
        "mysql"
      ],
      "image": "wordpress",
      "essential": true,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80
        }
      ],
      "memory": 500,
      "cpu": 10
    },
    {
      "environment": [
        {
          "name": "MYSQL_ROOT_PASSWORD",
          "value": "password"
        }
      ],
      "name": "mysql",
      "image": "mysql",
      "cpu": 10,
      "memory": 500,
      "essential": true
    }
  ],
  "family": "hello_world"
}
```

**Important**  
If you use this task definition with a load balancer, you need to complete the WordPress setup installation through the web interface on the container instance immediately after the container starts\. The load balancer health check ping expects a `200` response from the server, but WordPress returns a `301` until the installation is completed\. If the load balancer health check fails, the load balancer deregisters the instance\.

## Example: `awslogs` Log Driver<a name="example_task_definition-awslogs"></a>

The following example demonstrates how to use the `awslogs` log driver in a task definition that uses the Fargate launch type\. The `nginx` container sends its logs to the `ecs-log-streaming` log group in the `us-west-2` region\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.

```
{
  "containerDefinitions": [
    {
      "memory": 128,
      "portMappings": [
        {
          "hostPort": 80,
          "containerPort": 80,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "name": "nginx-container",
      "image": "nginx",
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "ecs-log-streaming",
          "awslogs-region": "us-west-2",
          "awslogs-stream-prefix": "fargate-task-1"
        }
      },
      "cpu": 0
    }
  ],
  "networkMode": "awsvpc",
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "memory": "2048",
  "cpu": "1024",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "family": "example_task_1"
}
```

## Example: `splunk` Log Driver<a name="example_task_definition-splunk"></a>

The following example demonstrates how to use the `splunk` log driver in a task definition that sends the logs to a remote service\. The Splunk token parameter is specified as a secret option because it can be treated as sensitive data\. For more information, see [Specifying Sensitive Data](specifying-sensitive-data.md)\.

```
"containerDefinitions": [{
		"logConfiguration": {
			"logDriver": "splunk",
			"options": {
				"splunk-url": "https://cloud.splunk.com:8080",
				"tag": "tag_name",
			},
			"secretOptions": [{
				"name": "splunk-token",
				"valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:splunk-token-KnrBkD"
}],
```

## Example: `fluentd` Log Driver<a name="example_task_definition-fluentd"></a>

The following example demonstrates how to use the `fluentd` log driver in a task definition that sends the logs to a remote service\. The `fluentd-address` value is specified as a secret option as it may be treated as sensitive data\. For more information, see [Specifying Sensitive Data](specifying-sensitive-data.md)\.

```
"containerDefinitions": [{
	"logConfiguration": {
		"logDriver": "fluentd",
		"options": {
			"tag": "fluentd demo"
		},
		"secretOptions": [{
			"name": "fluentd-address",
			"valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:fluentd-address-KnrBkD"
		}]
	},
	"entryPoint": [],
	"portMappings": [{
             "hostPort": 80,
             "protocol": "tcp",
             "containerPort": 80
             },
             {
		"hostPort": 24224,
		"protocol": "tcp",
		"containerPort": 24224
	}]
}],
```

## Example: `gelf` Log Driver<a name="example_task_definition-gelf"></a>

The following example demonstrates how to use the `gelf` log driver in a task definition that sends the logs to a remote host running Logstash that takes Gelf logs as an input\. For more information, see [logConfiguration](task_definition_parameters.md#ContainerDefinition-logConfiguration)\.

```
"containerDefinitions": [{
	"logConfiguration": {
		"logDriver": "gelf",
		"options": {
			"gelf-address": "udp://logstash-service-address:5000",
			"tag": "gelf task demo"
		}
	},
	"entryPoint": [],
	"portMappings": [{
			"hostPort": 5000,
			"protocol": "udp",
			"containerPort": 5000
		},
		{
			"hostPort": 5000,
			"protocol": "tcp",
			"containerPort": 5000
		}
	]
}],
```

## Example: Amazon ECR Image and Task Definition IAM Role<a name="example_task_definition-iam"></a>

The following example uses an Amazon ECR image called `aws-nodejs-sample` with the `v1` tag from the `123456789012.dkr.ecr.us-west-2.amazonaws.com` registry\. The container in this task inherits IAM permissions from the `arn:aws:iam::123456789012:role/AmazonECSTaskS3BucketRole` role\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

```
{
  "containerDefinitions": [
    {
      "name": "sample-app",
      "image": "123456789012.dkr.ecr.us-west-2.amazonaws.com/aws-nodejs-sample:v1",
      "memory": 200,
      "cpu": 10,
      "essential": true
    }
  ],
  "family": "example_task_3",
  "taskRoleArn": "arn:aws:iam::123456789012:role/AmazonECSTaskS3BucketRole"
}
```

## Example: Entrypoint with Command<a name="example_task_definition-ping"></a>

The following example demonstrates the syntax for a Docker container that uses an entry point and a command argument\. This container pings `google.com` four times and then exits\.

```
{
  "containerDefinitions": [
    {
      "memory": 32,
      "essential": true,
      "entryPoint": [
        "ping"
      ],
      "name": "alpine_ping",
      "readonlyRootFilesystem": true,
      "image": "alpine:3.4",
      "command": [
        "-c",
        "4",
        "google.com"
      ],
      "cpu": 16
    }
  ],
  "family": "example_task_2"
}
```

## Example: Container Dependency<a name="example_task_definition-containerdependency"></a>

This example demonstrates the syntax for a task definition with multiple containers where container dependency is specified\. In the following task definition, the `envoy` container must reach a healthy status, determined by the required container healthcheck parameters, before the `app` container will start\. For more information, see [Container Dependency](task_definition_parameters.md#container_definition_dependson)\.

```
{
  "family": "appmesh-gateway",
  "proxyConfiguration":{
      "type": "APPMESH",
      "containerName": "envoy",
      "properties": [
          {
              "name": "IgnoredUID",
              "value": "1337"
          },
          {
              "name": "ProxyIngressPort",
              "value": "15000"
          },
          {
              "name": "ProxyEgressPort",
              "value": "15001"
          },
          {
              "name": "AppPorts",
              "value": "9080"
          },
          {
              "name": "EgressIgnoredIPs",
              "value": "169.254.170.2,169.254.169.254"
          }
      ]
  },
  "containerDefinitions": [
    {
      "name": "app",
      "image": "application_image",
      "portMappings": [
        {
          "containerPort": 9080,
          "hostPort": 9080,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "dependsOn": [
        {
          "containerName": "envoy",
          "condition": "HEALTHY"
        }
      ]
    },
    {
      "name": "envoy",
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.12.2.1-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/meshName/virtualNode/virtualNodeName"
        },
        {
          "name": "ENVOY_LOG_LEVEL",
          "value": "info"
        }
      ],
      "healthCheck": {
        "command": [
          "CMD-SHELL",
          "echo hello"
        ],
        "interval": 5,
        "timeout": 2,
        "retries": 3
      }    
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc"
}
```