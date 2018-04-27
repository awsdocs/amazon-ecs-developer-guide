# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-1b90a67e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-1b90a67e) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-cb17d8b6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-cb17d8b6) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-05b5277d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-05b5277d) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-9cbbaffc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9cbbaffc) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-914afcec | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-914afcec) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-a48d6bc3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-a48d6bc3) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-bfb5fec6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-bfb5fec6) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-ac055447 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-ac055447) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-ba74d8d4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-ba74d8d4) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-5add893c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-5add893c) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-4cc5072e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-4cc5072e) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-acbcefd0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-acbcefd0) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-a535b2c1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-a535b2c1) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-2149114e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-2149114e) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-d3bce9bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-d3bce9bf) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.