# Amazon ECS Launch Types<a name="launch_types"></a>

An Amazon ECS launch type determines the type of infrastructure on which your tasks and services are hosted\.

## Fargate Launch Type<a name="launch-type-fargate"></a>

The Fargate launch type allows you to run your containerized applications without the need to provision and manage the backend infrastructure\. Just register your task definition and Fargate launches the container for you\.

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

For more information about Amazon ECS with AWS Fargate, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\.

## EC2 Launch Type<a name="launch-type-ec2"></a>

The EC2 launch type allows you to run your containerized applications on a cluster of Amazon EC2 instances that you manage\.

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)