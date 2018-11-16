# Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami"></a>

The Amazon ECS\-optimized Amazon Linux 2 AMI is the recommended AMI to use for launching your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized Amazon Linux 2 AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI\. We recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI ends no later than June 30, 2020\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.22.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.22.0-1`\)

The Amazon ECS\-optimized Amazon Linux 2 AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-037a92bf1efdb11a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-037a92bf1efdb11a2) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0a6b7e0cc0b1f464f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0a6b7e0cc0b1f464f) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0c1f4871ebaae6d86 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0c1f4871ebaae6d86) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0184f498956de7db5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0184f498956de7db5) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0caadc4f0db31a303 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0caadc4f0db31a303) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0b5225210a12d9951 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0b5225210a12d9951) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0acc9f8be17a41897 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0acc9f8be17a41897) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-055aa9664ef169e25 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-055aa9664ef169e25) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0bdc871079baf9649 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0bdc871079baf9649) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0c38293d60d98af86 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0c38293d60d98af86) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0eed1c915ea891aca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0eed1c915ea891aca) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0e28ff4e3f1776d86 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0e28ff4e3f1776d86) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-02c80e9173258d289 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-02c80e9173258d289) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-0b7c3be99909df6ef | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0b7c3be99909df6ef) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-078146697425f25a7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-078146697425f25a7) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181112\-x86\_64\-ebs | ami\-31b5d150 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-31b5d150) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami-get-latest.md)
+ [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)
+ [Storage Configuration](al2ami-storage-config.md)