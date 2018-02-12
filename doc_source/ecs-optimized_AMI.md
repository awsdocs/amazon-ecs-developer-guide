# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.h-amazon-ecs-optimized`\) consists of:

+ The latest minimal version of the Amazon Linux AMI

+ The latest version of the Amazon ECS container agent \(`1.17.0`\)

+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.09.1-ce`\)

+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.17.0-2`\)

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-67ab9e02 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-67ab9e02) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-5e414e24 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-5e414e24) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-10ed6968 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-10ed6968) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-00898660 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-00898660) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-6fa21412 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-6fa21412) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-42a64325 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-42a64325) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-880d64f1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-880d64f1) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-63cbae0c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-63cbae0c) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-0acc6e64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0acc6e64) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-e3166185 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-e3166185) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-36867d54 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-36867d54) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-66c98f1a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-66c98f1a) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-4b9c182f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-4b9c182f) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-ca8ad9a5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-ca8ad9a5) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.h\-amazon\-ecs\-optimized | ami\-69f7b805 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-69f7b805) | 


+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)