# Example task definitions<a name="example_task_definitions"></a>

This section provides some task definition examples that you can use to start creating your own task definitions\. For more information, see [Task definition parameters](task_definition_parameters.md) and [Creating a task definition](create-task-definition.md)\.

For additional task definition examples, see [AWS Sample Task Definitions](https://github.com/aws-samples/aws-containers-task-definitions) on GitHub\.

**Topics**
+ [Example: Webserver](#example_task_definition-webserver)
+ [Example: `splunk` log driver](#example_task_definition-splunk)
+ [Example: `fluentd` log driver](#example_task_definition-fluentd)
+ [Example: `gelf` log driver](#example_task_definition-gelf)
+ [Example: Amazon ECR image and task definition IAM role](#example_task_definition-iam)
+ [Example: Entrypoint with command](#example_task_definition-ping)
+ [Example: Container dependency](#example_task_definition-containerdependency)

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

## Example: `splunk` log driver<a name="example_task_definition-splunk"></a>

The following example demonstrates how to use the `splunk` log driver in a task definition that sends the logs to a remote service\. The Splunk token parameter is specified as a secret option because it can be treated as sensitive data\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.

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

## Example: `fluentd` log driver<a name="example_task_definition-fluentd"></a>

The following example demonstrates how to use the `fluentd` log driver in a task definition that sends the logs to a remote service\. The `fluentd-address` value is specified as a secret option as it may be treated as sensitive data\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.

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

## Example: `gelf` log driver<a name="example_task_definition-gelf"></a>

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

## Example: Amazon ECR image and task definition IAM role<a name="example_task_definition-iam"></a>

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

## Example: Entrypoint with command<a name="example_task_definition-ping"></a>

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

## Example: Container dependency<a name="example_task_definition-containerdependency"></a>

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
      "image": "840364872350.dkr.ecr.region-code.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
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