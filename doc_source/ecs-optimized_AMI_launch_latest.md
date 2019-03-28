# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 Console Link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized AMI ID, according to the variant you choose, below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

------
#### [ Amazon Linux 2 ]

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)

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

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-042479805229ff062 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-042479805229ff062) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0b057fabf316140f8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0b057fabf316140f8) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0ecd7efc5bd8b3ea9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0ecd7efc5bd8b3ea9) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190301\-arm64\-ebs | ami\-0ba589e5ce5223741 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0ba589e5ce5223741) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)
+ The recommended NVIDIA driver version \(`"NVIDIA Driver Version": "410.104"`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`"CUDA Version": "9.2.88"`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0c6e4820440f4cc0a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0c6e4820440f4cc0a) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0dfdeb4b6d47a87a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0dfdeb4b6d47a87a2) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-067476aa41f6df63c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-067476aa41f6df63c) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0afa05b0704a844e3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0afa05b0704a844e3) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-018dea12ea1283f1c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-018dea12ea1283f1c) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-04138059ccde89bf2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-04138059ccde89bf2) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0a6bf6559d7cf523b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0a6bf6559d7cf523b) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-038eff59f1fcbecd7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-038eff59f1fcbecd7) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0f1ec74fbd02df21a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0f1ec74fbd02df21a) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0b35c574ad7620303 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0b35c574ad7620303) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0b63b8d06ce4db732 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0b63b8d06ce4db732) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-05bcecc0d5b4a74bd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-05bcecc0d5b4a74bd) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0cac2b0201a03d81f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0cac2b0201a03d81f) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-094fa5a8ce4caf927 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-094fa5a8ce4caf927) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0cb807e1540647d46 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0cb807e1540647d46) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0d9e6af95335ea492 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0d9e6af95335ea492) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-0ac6ed4ded9a619dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0ac6ed4ded9a619dc) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190321\-x86\_64\-ebs | ami\-b4f98cd5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-b4f98cd5) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0cca5d0eeadc8c3c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0cca5d0eeadc8c3c4) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0a6a36557ea3b9859 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0a6a36557ea3b9859) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d2f82a622136a696 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0d2f82a622136a696) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-066a6b3ae13abc046 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-066a6b3ae13abc046) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-020cc3695affa4b6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-020cc3695affa4b6b) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0e1065cc4f7231034 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e1065cc4f7231034) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-00921cd1ce43d567a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-00921cd1ce43d567a) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-042ae7188819e7e9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-042ae7188819e7e9b) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0a92075786c5779b9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0a92075786c5779b9) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0e0d82e1272b5ae8a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0e0d82e1272b5ae8a) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-084cb340923dc7101 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-084cb340923dc7101) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-051b682e0d63cc816 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-051b682e0d63cc816) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0eb4239fe0f64fe58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0eb4239fe0f64fe58) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d9198a587e83919b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0d9198a587e83919b) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d7805fed18723d71 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0d7805fed18723d71) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0c4cd93b06ee26c34 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0c4cd93b06ee26c34) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-04a689185b06da6db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-04a689185b06da6db) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-7a5d361b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-7a5d361b) | 

------
#### [ Windows ]

The following table lists the current Amazon ECS\-optimized Windows AMI IDs by Region\.


| Region | AMI ID | 
| --- | --- | 
| us\-east\-2 | ami\-00c56e74f090d6f65 | 
| us\-east\-1 | ami\-0ed2f29599018e745 | 
| us\-west\-2 | ami\-07f6b98dc6c8067c3 | 
| us\-west\-1 | ami\-01b55f7fe967f727b | 
| eu\-west\-3 | ami\-06a5b6fc522511993 | 
| eu\-west\-2 | ami\-0fac4f3bdab9ccddc | 
| eu\-west\-1 | ami\-04a2fa8ce0fc20c61 | 
| eu\-central\-1 | ami\-0f7386282aa13a0d8 | 
| eu\-north\-1 | ami\-06e3cb4d2875b172e | 
| ap\-northeast\-2 | ami\-071b78467d9d35580 | 
| ap\-northeast\-1 | ami\-07839df9eec55ac8d | 
| ap\-southeast\-2 | ami\-06d33f81ca8384556 | 
| ap\-southeast\-1 | ami\-0a6c13d83c0fdbf2b | 
| ca\-central\-1 | ami\-098ad73a3005be676 | 
| ap\-south\-1 | ami\-0183732d8e0fd56c7 | 
| sa\-east\-1 | ami\-05889298c47e6d5c2 | 
| us\-gov\-east\-1 | ami\-0b6f703732ae49d69 | 
| us\-gov\-west\-1 | ami\-9d91fafc | 

------

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.