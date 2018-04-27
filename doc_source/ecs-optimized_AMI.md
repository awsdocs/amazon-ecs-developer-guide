# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.l-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.17.3`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.12.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.17.3-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

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

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)