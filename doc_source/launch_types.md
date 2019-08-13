# Amazon ECS Launch Types<a name="launch_types"></a>

An Amazon ECS launch type determines the type of infrastructure on which your tasks and services are hosted\.

## Fargate Launch Type<a name="launch-type-fargate"></a>

The Fargate launch type allows you to run your containerized applications without the need to provision and manage the backend infrastructure\. Just register your task definition and Fargate launches the container for you\.

The AWS Fargate launch type is currently available in the following Regions:


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
| AWS GovCloud \(US\-East\) | us\-gov\-east\-1 | 
| AWS GovCloud \(US\) | us\-gov\-west\-1 | 

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

For more information about Amazon ECS with AWS Fargate, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\.

## EC2 Launch Type<a name="launch-type-ec2"></a>

The EC2 launch type allows you to run your containerized applications on a cluster of Amazon EC2 instances that you manage\.

This diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)