# Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami"></a>

The Amazon ECS\-optimized Amazon Linux 2 AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized Amazon Linux 2 AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.21.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.21.0-1`\)

The Amazon ECS\-optimized Amazon Linux 2 AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-075d44ed0d20df780 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-075d44ed0d20df780) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0a6be20ed8ce1f055 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0a6be20ed8ce1f055) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-04a4fb062c609f55b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-04a4fb062c609f55b) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0828cd2f20543a2b3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0828cd2f20543a2b3) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0b0b5fca56d03d022 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0b0b5fca56d03d022) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0ba06c0257c11d483 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0ba06c0257c11d483) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0651de2fa6ccf6d26 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0651de2fa6ccf6d26) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-07c3a868617f8acdb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-07c3a868617f8acdb) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0fcea96f3b2c67274 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0fcea96f3b2c67274) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-05a1f057a1a8e1a50 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-05a1f057a1a8e1a50) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0231cd5b17565f0b1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0231cd5b17565f0b1) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0d4d4a42a45fb8e4a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0d4d4a42a45fb8e4a) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0605f253b9e04698c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0605f253b9e04698c) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-00afc9e848e32b1ca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-00afc9e848e32b1ca) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-0f2187a18380957e4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0f2187a18380957e4) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181017\-x86\_64\-ebs | ami\-19cd5778 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-19cd5778) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami-get-latest.md)
+ [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)
+ [Storage Configuration](al2ami-storage-config.md)