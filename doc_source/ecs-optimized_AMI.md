# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.a-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.18.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.12.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.18.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

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

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)