# Amazon ECS launch types<a name="launch_types"></a>

You can specify an Amazon ECS launch type when you run a standalone task or create a service\. Doing so determines the infrastructure that your tasks and services are hosted on\. The following are the available launch types\.

## Fargate launch type<a name="launch-type-fargate"></a>

You can use the Fargate launch type to run your containerized applications without the need of provisioning and managing the underlying infrastructure\. AWS Fargate is the serverless way to host your Amazon ECS workloads\.

For information about the Regions that support Fargate, see [Supported Regions for Amazon ECS on AWS Fargate](AWS_Fargate-Regions.md)\.

The following diagram shows the general architecture\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

For more information about Amazon ECS on Fargate, see [Amazon ECS on AWS Fargate](AWS_Fargate.md)\.

## EC2 launch type<a name="launch-type-ec2"></a>

You can use the EC2 launch type to run your containerized applications on Amazon EC2 instances that you register to your Amazon ECS cluster and manage yourself\.

The following diagram shows the general architecture\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)

## External launch type<a name="launch-type-external"></a>

The External launch type is used to run your containerized applications on your on\-premise server or virtual machine \(VM\) that you register to your Amazon ECS cluster and manage remotely\. For more information, see [External instances \(Amazon ECS Anywhere\)](ecs-anywhere.md)\.