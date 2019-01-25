# Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami"></a>

The Amazon ECS\-optimized Amazon Linux 2 AMI is the recommended AMI to use for launching your Amazon ECS container instances\. Amazon ECS provides separate Amazon ECS\-optimized Amazon Linux 2 AMIs for x86 and arm64 architecture\.

Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized Amazon Linux 2 AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI\. We recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI ends no later than June 30, 2020\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.25.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.25.0-1`\)

The Amazon ECS\-optimized Amazon Linux 2 AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-093b710aa3d88540d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-093b710aa3d88540d) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-014810e050a2986e9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-014810e050a2986e9) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0bd5f1e6656ee98d6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0bd5f1e6656ee98d6) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0ed6989bdf821b0fb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0ed6989bdf821b0fb) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0d86fa8483cfc9a34 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0d86fa8483cfc9a34) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0b48a90115318e01c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0b48a90115318e01c) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-059d1b1771487e7a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-059d1b1771487e7a2) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-016b7db231a5906da | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-016b7db231a5906da) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0bb17cf9f4c34ccde | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0bb17cf9f4c34ccde) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-076742548207a9869 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-076742548207a9869) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-06884bb5517cd1ec5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-06884bb5517cd1ec5) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0d5df8acd1ff68422 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0d5df8acd1ff68422) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-06534f8692c419582 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-06534f8692c419582) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-06956934e43c09088 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-06956934e43c09088) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0b69100abbf75258c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0b69100abbf75258c) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-0cbc976a4d63b3523 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0cbc976a4d63b3523) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190118\-x86\_64\-ebs | ami\-4b8be62a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-4b8be62a) | 

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190119\-arm64\-ebs | ami\-03199f44dfc5fabbe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-03199f44dfc5fabbe) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190119\-arm64\-ebs | ami\-0e373a2446b389bf4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0e373a2446b389bf4) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190119\-arm64\-ebs | ami\-0f97835ab144d7db8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0f97835ab144d7db8) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190119\-arm64\-ebs | ami\-06b2fbdf41d3f6490 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-06b2fbdf41d3f6490) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami-get-latest.md)
+ [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)
+ [Storage Configuration](al2ami-storage-config.md)