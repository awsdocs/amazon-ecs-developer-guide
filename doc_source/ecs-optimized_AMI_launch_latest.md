# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ To launch your instances, use the current Amazon ECS\-optimized AMI to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e65e665ff5f3fc5f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0e65e665ff5f3fc5f) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-112e366e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-112e366e) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-a1f8dfd9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-a1f8dfd9) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-dd0de2be | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-dd0de2be) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e4185127a627bbac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0e4185127a627bbac) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-c3ea1fa4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-c3ea1fa4) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0612d1ef7f8e72c06 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0612d1ef7f8e72c06) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-c7e9e72c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-c7e9e72c) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-02b0706448bd6fb5e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-02b0706448bd6fb5e) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-256c15c8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-256c15c8) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-df49e9bd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-df49e9bd) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e1566e9c8eb85002 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0e1566e9c8eb85002) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-cf60edab | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-cf60edab) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0c814df738f3c9fd5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0c814df738f3c9fd5) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-085384d0e5fd5ae0a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-085384d0e5fd5ae0a) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-8b8814ea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-8b8814ea) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.