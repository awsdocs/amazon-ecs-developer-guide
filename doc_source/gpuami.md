# Amazon ECS GPU\-Optimized AMI<a name="gpuami"></a>

The Amazon ECS GPU\-optimized AMI is the recommended AMI to use for launching your Amazon ECS container instances when working with GPU workloads\.

Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS GPU\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It's the simplest AMI for you to get started with and to get your containers running on AWS quickly\.

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.25.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.25.1-1`\)
+ The recommended NVIDIA driver version \(`"NVIDIA Driver Version": "396.26"`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`"CUDA Version": "9.2.88"`\)

You can programmatically retrieve the Amazon ECS GPU\-optimized AMI metadata, including the AMI ID\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-019b7397ab7894359 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-019b7397ab7894359) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0627f27e8926eb40b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0627f27e8926eb40b) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0eec89cf14d0fd2dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0eec89cf14d0fd2dc) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0f3e8e0bb0d52bf7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0f3e8e0bb0d52bf7d) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0c19c6dc25194f8c2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0c19c6dc25194f8c2) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-095672e42797fa58b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-095672e42797fa58b) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-057545dc4ad11a331 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-057545dc4ad11a331) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0132cafd92b44fe89 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0132cafd92b44fe89) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0e90517658218d086 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0e90517658218d086) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0ecdf6197f6f8719c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0ecdf6197f6f8719c) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0988436332d0dd80b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0988436332d0dd80b) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-0dca378e57d8069cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0dca378e57d8069cb) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-022d42ecc9e16d7a0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-022d42ecc9e16d7a0) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-04ca24b3c0baf7e14 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-04ca24b3c0baf7e14) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-09481290ccf9528f7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-09481290ccf9528f7) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-024d71a0469765af1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-024d71a0469765af1) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190127\-x86\_64\-ebs | ami\-d20d61b3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-d20d61b3) | 

**Topics**
+ [How to Launch the Latest Amazon ECS GPU\-Optimized AMI](gpuami-get-latest.md)
+ [Amazon ECS GPU\-Optimized AMI Versions](gpuami-agent-versions.md)
+ [Storage Configuration](gpuami-storage-config.md)