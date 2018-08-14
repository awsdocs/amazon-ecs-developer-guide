# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-028a9de0a7e353ed9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-028a9de0a7e353ed9) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-00129b193dc81bc31 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-00129b193dc81bc31) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-00d4f478 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-00d4f478) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0d438d09af26c9583 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0d438d09af26c9583) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-07da674f0655ef4e1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-07da674f0655ef4e1) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-a44db8c3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-a44db8c3) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0af844a965e5738db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0af844a965e5738db) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0291ba887ba0d515f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0291ba887ba0d515f) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-047d2a61f94f862dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-047d2a61f94f862dc) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0041c416aa23033a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0041c416aa23033a2) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0092e55c70015d8c3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0092e55c70015d8c3) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-091bf462afdb02c60 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-091bf462afdb02c60) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-192fa27d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-192fa27d) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0c179ca015d301829 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0c179ca015d301829) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-0018ff8ee48970ac3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0018ff8ee48970ac3) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.e\-amazon\-ecs\-optimized | ami\-c6079ba7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-c6079ba7) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.