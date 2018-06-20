# Example Task Definitions<a name="example_task_definitions"></a>

Below are some task definition examples that you can use to start creating your own task definitions\. For more information, see [Task Definition Parameters](task_definition_parameters.md) and [Creating a Task Definition](create-task-definition.md)\.

**Topics**

**Example Example: WordPress and MySQL**  <a name="example_task_definition-wordpress"></a>
The following example specifies a WordPress container and a MySQL container that are linked together\. This WordPress container exposes the container port 80 on the host port 80\. The security group on the container instance would need to open port 80 in order for this WordPress installation to be accessible from a web browser\.  
For more information about the WordPress container, go to the official WordPress Docker Hub repository at [https://registry\.hub\.docker\.com/\_/wordpress/](https://registry.hub.docker.com/_/wordpress/)\. For more information about the MySQL container, go to the official MySQL Docker Hub repository at [https://registry\.hub\.docker\.com/\_/mysql/](https://registry.hub.docker.com/_/mysql/)\.  

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

**Example Example: `awslogs` Log Driver**  <a name="example_task_definition-awslogs"></a>
The following example demonstrates how to use the `awslogs` log driver in a task definition\. The `nginx` container will send its logs to the `ecs-log-streaming` log group in the `us-west-2` region\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.  

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
          "awslogs-region": "us-west-2"
        }
      },
      "cpu": 0
    }
  ],
  "family": "example_task_1"
}
```

**Example Example: Amazon ECR Image and Task Definition IAM Role**  <a name="example_task_definition-iam"></a>
The following example uses an Amazon ECR image called `aws-nodejs-sample` with the `v1` tag from the `123456789012.dkr.ecr.us-west-2.amazonaws.com` registry\. The container in this task will inherit IAM permissions from the `arn:aws:iam::123456789012:role/AmazonECSTaskS3BucketRole` role\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.  

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

**Example Example: Entrypoint with Command**  <a name="example_task_definition-ping"></a>
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