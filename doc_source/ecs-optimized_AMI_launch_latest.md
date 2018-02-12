# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ Use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template to launch your instances\. 

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-67ab9e02 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-67ab9e02) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-5e414e24 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-5e414e24) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-10ed6968 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-10ed6968) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-00898660 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-00898660) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-6fa21412 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-6fa21412) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-42a64325 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-42a64325) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-880d64f1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-880d64f1) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-63cbae0c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-63cbae0c) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-0acc6e64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0acc6e64) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-e3166185 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-e3166185) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-36867d54 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-36867d54) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-66c98f1a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-66c98f1a) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-4b9c182f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-4b9c182f) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-ca8ad9a5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-ca8ad9a5) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-69f7b805 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-69f7b805) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.