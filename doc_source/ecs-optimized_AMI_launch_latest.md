# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-79d8e21c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-79d8e21c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-644a431b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-644a431b) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-f189d189 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-f189d189) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-4351bc20 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-4351bc20) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-e976c694 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-e976c694) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-2e9866c5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-2e9866c5) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-39d530d4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-39d530d4) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-9fe2e074 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-9fe2e074) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-8f44f3e1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-8f44f3e1) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-7d0c7a90 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-7d0c7a90) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-706cca12 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-706cca12) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-ae1b5a44 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-ae1b5a44) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-c1b63ba5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-c1b63ba5) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-f4b88a9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-f4b88a9b) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-a2c6e7ce | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-a2c6e7ce) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-ceda47af | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-ceda47af) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.