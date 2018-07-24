# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-8f4e74ea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-8f4e74ea) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-fbc1c684 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-fbc1c684) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-7ddf8005 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-7ddf8005) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-638c6100 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-638c6100) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2187375c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-2187375c) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-e8a04a8f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-e8a04a8f) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-74e7fe9e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-74e7fe9e) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-c123232a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-c123232a) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2095224e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-2095224e) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2b4d26c6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-2b4d26c6) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-09bf186b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-09bf186b) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-3b1d59d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-3b1d59d1) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-06ac2162 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-06ac2162) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-921d2efd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-921d2efd) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-0a8caa66 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0a8caa66) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-5147d530 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-5147d530) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.