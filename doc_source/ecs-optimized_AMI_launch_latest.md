# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-956e52f0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-956e52f0) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-5253c32d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-5253c32d) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-d2f489aa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-d2f489aa) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-6b81980b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-6b81980b) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-ca75c4b7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-ca75c4b7) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-3622cf51 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-3622cf51) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-c91624b0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-c91624b0) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-10e6c8fb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-10e6c8fb) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-7c69c112 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-7c69c112) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-f3f8098c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-f3f8098c) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-bc04d5de | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-bc04d5de) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-b75a6acb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-b75a6acb) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-da6cecbe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-da6cecbe) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-c7072aa8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-c7072aa8) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.a\-amazon\-ecs\-optimized | ami\-a1e2becd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-a1e2becd) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.