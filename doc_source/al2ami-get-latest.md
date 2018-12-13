# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami-get-latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized Amazon Linux 2 AMI into your cluster:
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized Amazon Linux 2 AMI ID below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

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

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181120\-arm64\-ebs | ami\-0a2be55320fc4a274 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0a2be55320fc4a274) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181120\-arm64\-ebs | ami\-0e851dee3d33f685e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0e851dee3d33f685e) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181120\-arm64\-ebs | ami\-053b2a8c2f3e87928 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-053b2a8c2f3e87928) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20181120\-arm64\-ebs | ami\-0874eb7dff7483ab5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0874eb7dff7483ab5) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)\.