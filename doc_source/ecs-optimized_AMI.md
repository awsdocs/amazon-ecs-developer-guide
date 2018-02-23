# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2017.09.i-amazon-ecs-optimized`\) consists of:

+ The latest minimal version of the Amazon Linux AMI

+ The latest version of the Amazon ECS container agent \(`1.17.1`\)

+ The recommended version of Docker for the latest Amazon ECS container agent \(`17.09.1-ce`\)

+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.17.1-1`\)

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-b86a5ddd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-b86a5ddd) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-a7a242da | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-a7a242da) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-92e06fea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-92e06fea) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-9ad4dcfa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-9ad4dcfa) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-698b3d14 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-698b3d14) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-f4e20693 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-f4e20693) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0693ed7f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0693ed7f) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0799fa68 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0799fa68) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-a5dd70cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-a5dd70cb) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-68ef940e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-68ef940e) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-ee884f8c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-ee884f8c) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-0a622c76 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0a622c76) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-5ac94e3e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-5ac94e3e) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-2e461a41 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-2e461a41) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.i\-amazon\-ecs\-optimized | ami\-d44008b8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-d44008b8) | 


+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)