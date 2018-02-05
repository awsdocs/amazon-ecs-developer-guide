# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ Use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template to launch your instances\. 

The current Amazon ECS–optimized AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-58f5db3d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-58f5db3d) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-fad25980 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-fad25980) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-7114c909 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-7114c909) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-62e0d802 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-62e0d802) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-dbfee1bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-dbfee1bf) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-4cbe0935 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-4cbe0935) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-05991b6a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-05991b6a) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-7267c01c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-7267c01c) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-56bd0030 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-56bd0030) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-14b55f76 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-14b55f76) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-1bdc8b78 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-1bdc8b78) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-918b30f5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-918b30f5) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.