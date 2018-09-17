# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0a0d2004b44b9287c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0a0d2004b44b9287c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0254e5972ebcd132c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0254e5972ebcd132c) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-093381d21a4fc38d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-093381d21a4fc38d1) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0de5608ca20c07aa2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0de5608ca20c07aa2) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-024c0b7d07abc6526 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-024c0b7d07abc6526) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-005307409c5f6e76c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-005307409c5f6e76c) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0dbcd2533bc72c3f6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0dbcd2533bc72c3f6) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-03804565a6baf6d30 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-03804565a6baf6d30) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0060ad36f655af38b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0060ad36f655af38b) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0d5f884dada5562c6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0d5f884dada5562c6) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0aa8b7a8042811ddf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0aa8b7a8042811ddf) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-065c0bd2832a70f9d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-065c0bd2832a70f9d) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0d50dee936e241e7e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0d50dee936e241e7e) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-056a07eb5b1d13734 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-056a07eb5b1d13734) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0078e33a9103e1e58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0078e33a9103e1e58) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-a842dcc9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-a842dcc9) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.