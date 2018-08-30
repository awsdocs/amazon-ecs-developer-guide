# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.e-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.20.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.03.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.20.1-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

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

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)