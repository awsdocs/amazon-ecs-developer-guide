# Amazon ECS Launch Types<a name="launch_types"></a>

An Amazon ECS launch type determines the type of infrastructure on which your tasks and services are hosted\.

## Fargate Launch Type<a name="launch-type-fargate"></a>

The Fargate launch type allows you to run your containerized applications without the need to provision and manage the backend infrastructure\. Just register your task definition and Fargate launches the container for you\.

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

If you use the Fargate launch type, the following task parameters are not valid:

+ `dockerSecurityOptions`

+ `links`

+ `linuxParameters`

+ `placementConstraints`

+ `privileged`

If you use the Fargate launch type, the following task parameters can be used but with limitations:

+ `networkMode` ‐ The only valid value is `awsvpc`\. For more information, see [Network Mode](task_definition_parameters.md#network_mode)\.

+ `portMappings` ‐ You should specify any exposed ports as `containerPort`\. The `hostPort` can be left blank\.

+ `logConfiguration` ‐ The only valid value is `awslogs`\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.

+ `volumes` ‐ The `host` and `sourcePath` values are not valid\. There are also specific service limits related to volumes for tasks using the Fargate launch type\. For more information, see [Amazon ECS Service Limits](service_limits.md)\.

+ There are separate task definition parameters for container and task size\. The container size parameters are optional\. The task size parameters are required and have specific values that must be used\. For more information, see [Task Size](task_definition_parameters.md#task_size)\.

## EC2 Launch Type<a name="launch-type-ec2"></a>

The EC2 launch type allows you to run your containerized applications on a cluster of Amazon EC2 instances that you manage\.

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)