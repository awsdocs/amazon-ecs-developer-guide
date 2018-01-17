# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ Use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template to launch your instances\. 

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-13af8476 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-13af8476) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-ba722dc0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-ba722dc0) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-c9c87cb1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-c9c87cb1) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-9df0f0fd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9df0f0fd) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-5e02b523 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-5e02b523) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-4d809829 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-4d809829) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-acb020d5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-acb020d5) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-eacf5d85 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-eacf5d85) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-59b71737 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-59b71737) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-72f36a14 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-72f36a14) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-7aa15c18 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-7aa15c18) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-e782f29b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-e782f29b) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-9afc79fe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-9afc79fe) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-f4db8f9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-f4db8f9b) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-49256725 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-49256725) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.