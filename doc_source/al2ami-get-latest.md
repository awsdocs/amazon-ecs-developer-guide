# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami-get-latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized Amazon Linux 2 AMI into your cluster:
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized Amazon Linux 2 AMI ID below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0fbc9fff39b859770 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0fbc9fff39b859770) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-00a0ec1744b47e7e3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-00a0ec1744b47e7e3) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0a8893b3446d3e884 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0a8893b3446d3e884) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-09a718d703d4dfd1d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-09a718d703d4dfd1d) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0d8b13ba7c91ce621 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0d8b13ba7c91ce621) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0e57cd83d692ab020 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e57cd83d692ab020) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-07c916f51e03b3fc6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-07c916f51e03b3fc6) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-00c89894d831fd9bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-00c89894d831fd9bf) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0b1735736271ee063 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0b1735736271ee063) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0309114e6e5df021e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0309114e6e5df021e) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-079d813d04571915e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-079d813d04571915e) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-03ba7b79573cdbe5e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-03ba7b79573cdbe5e) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-06227143764cbe5b6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-06227143764cbe5b6) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0b8d05f39cf05bdb9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0b8d05f39cf05bdb9) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0d4c0e46eb46325cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0d4c0e46eb46325cb) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-0aaea9f335c45e3a3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0aaea9f335c45e3a3) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-x86\_64\-ebs | ami\-843351e5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-843351e5) | 

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-arm64\-ebs | ami\-0283be7c51553d21a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0283be7c51553d21a) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-arm64\-ebs | ami\-050fdacbc39dffd05 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-050fdacbc39dffd05) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-arm64\-ebs | ami\-0fdba42282afd8d70 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0fdba42282afd8d70) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190107\-arm64\-ebs | ami\-0b86e0124e957c7aa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0b86e0124e957c7aa) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)\.