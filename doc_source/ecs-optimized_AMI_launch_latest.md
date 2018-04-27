# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-64300001 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-64300001) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-aff65ad2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-aff65ad2) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-40ddb938 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-40ddb938) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-69677709 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-69677709) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-250eb858 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-250eb858) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-2218f945 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-2218f945) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-2d386654 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-2d386654) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-9fc39c74 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-9fc39c74) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-9d56f9f3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-9d56f9f3) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-a99d8ad5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-a99d8ad5) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-efda148d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-efda148d) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-846144f8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-846144f8) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-897ff9ed | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-897ff9ed) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-72edc81d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-72edc81d) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.l\-amazon\-ecs\-optimized | ami\-4a7e2826 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-4a7e2826) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.