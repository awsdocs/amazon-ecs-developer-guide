# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.f-amazon-ecs-optimized`\) consists of:

+ The latest minimal version of the Amazon Linux AMI

+ The latest version of the Amazon ECS container agent \(`1.16.1`\)

+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.06.2-ce`\)

+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.16.1-1`\)

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-13af8476 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-13af8476) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-ba722dc0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-ba722dc0) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-c9c87cb1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-c9c87cb1) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-9df0f0fd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9df0f0fd) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-5e02b523 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-5e02b523) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-4d809829 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-4d809829) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-acb020d5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-acb020d5) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-eacf5d85 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-eacf5d85) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-59b71737 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-59b71737) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-72f36a14 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-72f36a14) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-7aa15c18 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-7aa15c18) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-e782f29b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-e782f29b) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-9afc79fe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-9afc79fe) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-f4db8f9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-f4db8f9b) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.f\-amazon\-ecs\-optimized | ami\-49256725 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-49256725) | 


+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)