# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.k-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.17.2`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.12.0-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.17.2-1`\)

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-1b90a67e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-1b90a67e) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-cb17d8b6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-cb17d8b6) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-05b5277d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-05b5277d) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-9cbbaffc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9cbbaffc) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-914afcec | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-914afcec) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-a48d6bc3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-a48d6bc3) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-bfb5fec6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-bfb5fec6) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-ac055447 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-ac055447) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-ba74d8d4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-ba74d8d4) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-5add893c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-5add893c) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-4cc5072e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-4cc5072e) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-acbcefd0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-acbcefd0) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-a535b2c1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-a535b2c1) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-2149114e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-2149114e) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.k\-amazon\-ecs\-optimized | ami\-d3bce9bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-d3bce9bf) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)