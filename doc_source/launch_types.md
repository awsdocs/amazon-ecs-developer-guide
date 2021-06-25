# Amazon ECS launch types<a name="launch_types"></a>

An Amazon ECS launch type can be specified when running a standalone task or creating a service to determine the infrastructure on which your tasks and services are hosted\. The following are the available launch types\.

## Fargate launch type<a name="launch-type-fargate"></a>

The Fargate launch type can be used to run your containerized applications without the need to provision and manage the backend infrastructure\. AWS Fargate is the serverless way to host your Amazon ECS workloads\.

The AWS Fargate launch type is currently available in the following Regions:


| Region Name | Region | 
| --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  US West \(N\. California\)  |  us\-west\-1 \(`usw1-az1` & `usw1-az3` only\)  | 
|  US West \(Oregon\)  |  us\-west\-2  | 
|  Africa \(Cape Town\)  |  af\-south\-1  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | 
|  Asia Pacific \(Osaka\)  |  ap\-northeast\-3  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1 \(`apne1-az1`, `apne1-az2`, & `apne1-az4` only\)  | 
|  Canada \(Central\)  |  ca\-central\-1 \(`cac1-az1` & `cac1-az2` only\)  | 
|  China \(Beijing\)  |  cn\-north\-1 \(`cnn1-az1` & `cnn1-az2` only\)  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | 
|  Europe \(London\)  |  eu\-west\-2  | 
|  Europe \(Paris\)  |  eu\-west\-3  | 
|  Europe \(Milan\)  |  eu\-south\-1  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | 
|  South America \(São Paulo\)  |  sa\-east\-1  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  | 

The following diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

For more information about Amazon ECS on Fargate, see [Amazon ECS on AWS Fargate](AWS_Fargate.md)\.

## EC2 launch type<a name="launch-type-ec2"></a>

The EC2 launch type can be used to run your containerized applications on Amazon EC2 instances that you register to your Amazon ECS cluster and manage yourself\.

The following diagram shows the general architecture:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-standard.png)

## External launch type<a name="launch-type-external"></a>

The External launch type is used to run your containerized applications on your on\-premise server or virtual machine \(VM\) that you register to your Amazon ECS cluster and manage remotely\. For more information, see [External instances \(Amazon ECS Anywhere\)](ecs-anywhere.md)\.