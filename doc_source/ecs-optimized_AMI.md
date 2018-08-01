# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

<<<<<<< HEAD
The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.c-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.19.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.03.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.19.1-1`\)
=======
The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.b-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.19.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.03.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.19.0-1`\)
>>>>>>> 674707ca5af33aacc808f1a019859ba993c8b0a0

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
<<<<<<< HEAD
| us\-east\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-79d8e21c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-79d8e21c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-644a431b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-644a431b) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-f189d189 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-f189d189) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-4351bc20 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-4351bc20) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-e976c694 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-e976c694) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-2e9866c5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-2e9866c5) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-39d530d4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-39d530d4) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-9fe2e074 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-9fe2e074) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-8f44f3e1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-8f44f3e1) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-7d0c7a90 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-7d0c7a90) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-706cca12 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-706cca12) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-ae1b5a44 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-ae1b5a44) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-c1b63ba5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-c1b63ba5) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-f4b88a9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-f4b88a9b) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-a2c6e7ce | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-a2c6e7ce) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.c\-amazon\-ecs\-optimized | ami\-ceda47af | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-ceda47af) | 
=======
| us\-east\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-8f4e74ea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-8f4e74ea) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-fbc1c684 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-fbc1c684) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-7ddf8005 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-7ddf8005) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-638c6100 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-638c6100) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2187375c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-2187375c) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-e8a04a8f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-e8a04a8f) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-74e7fe9e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-74e7fe9e) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-c123232a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-c123232a) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2095224e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-2095224e) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-2b4d26c6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-2b4d26c6) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-09bf186b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-09bf186b) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-3b1d59d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-3b1d59d1) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-06ac2162 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-06ac2162) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-921d2efd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-921d2efd) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-0a8caa66 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0a8caa66) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.b\-amazon\-ecs\-optimized | ami\-5147d530 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-5147d530) | 
>>>>>>> 674707ca5af33aacc808f1a019859ba993c8b0a0

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)