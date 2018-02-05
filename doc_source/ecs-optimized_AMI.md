# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.d-amazon-ecs-optimized`\) consists of:

+ The latest minimal version of the Amazon Linux AMI

+ The latest version of the Amazon ECS container agent \(`1.16.0`\)

+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.06.2-ce`\)

+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.16.0-1`\)

The current Amazon ECS–optimized AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-58f5db3d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-58f5db3d) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-fad25980 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-fad25980) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-7114c909 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-7114c909) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-62e0d802 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-62e0d802) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-dbfee1bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-dbfee1bf) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-4cbe0935 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-4cbe0935) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-05991b6a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-05991b6a) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-7267c01c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-7267c01c) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-56bd0030 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-56bd0030) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-14b55f76 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-14b55f76) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-1bdc8b78 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-1bdc8b78) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.d\-amazon\-ecs\-optimized | ami\-918b30f5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-918b30f5) | 


+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)