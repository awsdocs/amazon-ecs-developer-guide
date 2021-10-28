# Amazon ECS launch types<a name="launch_types"></a>

An Amazon ECS launch type can be specified when running a standalone task or creating a service to determine the infrastructure on which your tasks and services are hosted\. The following are the available launch types\.

## Fargate launch type<a name="launch-type-fargate"></a>

The Fargate launch type can be used to run your containerized applications without the need to provision and manage the backend infrastructure\. AWS Fargate is the serverless way to host your Amazon ECS workloads\.

For information about the Regions that support Fargate, see [Supported Regions for Amazon ECS on AWS Fargate](AWS_Fargate-Regions.md)\.

The following diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

For more information about Amazon ECS on Fargate, see [Amazon ECS on AWS Fargate](AWS_Fargate.md)\.

## EC2 launch type<a name="launch-type-ec2"></a>

The EC2 launch type can be used to run your containerized applications on Amazon EC2 instances that you register to your Amazon ECS cluster and manage yourself\.

The following diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)

## External launch type<a name="launch-type-external"></a>

The External launch type is used to run your containerized applications on your on\-premise server or virtual machine \(VM\) that you register to your Amazon ECS cluster and manage remotely\. For more information, see [External instances \(Amazon ECS Anywhere\)](ecs-anywhere.md)\.