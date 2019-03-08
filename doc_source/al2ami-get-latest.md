# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI<a name="al2ami-get-latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized Amazon Linux 2 AMI into your cluster:
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized Amazon Linux 2 AMI ID below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0aa9ee1fc70e57450 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0aa9ee1fc70e57450) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-007571470797b8ffa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-007571470797b8ffa) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0302f3ec240b9d23c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0302f3ec240b9d23c) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0935a5e8655c6d896 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0935a5e8655c6d896) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0b419de35e061d9df | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0b419de35e061d9df) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0380c676fcff67fd5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0380c676fcff67fd5) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0b8e62ddc09226d0a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0b8e62ddc09226d0a) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-01b63d839941375df | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-01b63d839941375df) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-03f8f3eb89dcfe553 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-03f8f3eb89dcfe553) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0c57dafd95a102862 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0c57dafd95a102862) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-086ca990ae37efc1b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-086ca990ae37efc1b) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0d28e5e0f13248294 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0d28e5e0f13248294) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0627e2913cf6756ed | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0627e2913cf6756ed) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-0835b198c8a7aced4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0835b198c8a7aced4) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-05de310b944d67cde | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-05de310b944d67cde) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-09987452123fadc5b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-09987452123fadc5b) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-07dfc9cdc48d8649a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-07dfc9cdc48d8649a) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-x86\_64\-ebs | ami\-914229f0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-914229f0) | 

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-042479805229ff062 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-042479805229ff062) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0b057fabf316140f8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0b057fabf316140f8) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0ecd7efc5bd8b3ea9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0ecd7efc5bd8b3ea9) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0ba589e5ce5223741 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0ba589e5ce5223741) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](al2ami-agent-versions.md)\.