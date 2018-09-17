# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.f-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.20.2`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.20.2-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

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

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)