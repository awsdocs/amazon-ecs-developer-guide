# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ To launch your instances, use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\. 

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-ef64528a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-ef64528a) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-cad827b7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-cad827b7) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-baa236c2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-baa236c2) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-29b8b249 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-29b8b249) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-0356e07e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0356e07e) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-25f51242 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-25f51242) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-64c4871d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-64c4871d) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-3b7d1354 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-3b7d1354) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-3b19b455 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-3b19b455) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-bb5f13dd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-bb5f13dd) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-a677b6c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-a677b6c4) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-f88ade84 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-f88ade84) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-db48cfbf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-db48cfbf) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-9e91cff1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-9e91cff1) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized | ami\-da2c66b6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-da2c66b6) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.