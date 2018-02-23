# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ Use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template to launch your instances\. 

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-b86a5ddd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-b86a5ddd) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-a7a242da | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-a7a242da) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-92e06fea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-92e06fea) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-9ad4dcfa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9ad4dcfa) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-698b3d14 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-698b3d14) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-f4e20693 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-f4e20693) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0693ed7f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0693ed7f) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0799fa68 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0799fa68) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-a5dd70cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-a5dd70cb) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-68ef940e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-68ef940e) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-ee884f8c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-ee884f8c) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0a622c76 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0a622c76) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-5ac94e3e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-5ac94e3e) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-2e461a41 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-2e461a41) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-d44008b8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-d44008b8) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.