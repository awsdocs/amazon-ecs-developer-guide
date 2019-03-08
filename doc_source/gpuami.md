# Amazon ECS GPU\-Optimized AMI<a name="gpuami"></a>

The Amazon ECS GPU\-optimized AMI is the recommended AMI to use for launching your Amazon ECS container instances when working with GPU workloads\.

Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS GPU\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It's the simplest AMI for you to get started with and to get your containers running on AWS quickly\.

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)
+ The recommended NVIDIA driver version \(`"NVIDIA Driver Version": "396.26"`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`"CUDA Version": "9.2.88"`\)

You can programmatically retrieve the Amazon ECS GPU\-optimized AMI metadata, including the AMI ID\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0842876a88f77484a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0842876a88f77484a) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0ebf2c738e66321e6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0ebf2c738e66321e6) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-09eed60087d68b913 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-09eed60087d68b913) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-05270086ae1b3419a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-05270086ae1b3419a) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0779e8a21ed1ae7f0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0779e8a21ed1ae7f0) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0d2450232adce5b42 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0d2450232adce5b42) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-068a212fd35f9875d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-068a212fd35f9875d) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-03b3a17229b1f591d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-03b3a17229b1f591d) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-03c32d2ab970ac49e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-03c32d2ab970ac49e) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-06b81cf321faac190 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-06b81cf321faac190) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0e89f600b3e9b249f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e89f600b3e9b249f) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-046e83f16f6a0ad75 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-046e83f16f6a0ad75) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-01c097f7b4a4e3f6e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-01c097f7b4a4e3f6e) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-09f948fe887ddb9b1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-09f948fe887ddb9b1) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0ae68f24ed5b07baf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0ae68f24ed5b07baf) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0a695d1ccc2653e9a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0a695d1ccc2653e9a) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-06c5c34e1d78fc831 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-06c5c34e1d78fc831) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-e9462d88 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-e9462d88) | 

**Topics**
+ [How to Launch the Latest Amazon ECS GPU\-Optimized AMI](gpuami-get-latest.md)
+ [Amazon ECS GPU\-Optimized AMI Versions](gpuami-agent-versions.md)
+ [Storage Configuration](gpuami-storage-config.md)