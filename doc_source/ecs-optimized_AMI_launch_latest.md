# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 Console Link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the Systems Manager API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized AMI ID, according to the variant you choose, below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

------
#### [ Amazon Linux 2 ]

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.29.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0329a1fdc914b0c55 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0329a1fdc914b0c55) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-02507631a9f7bc956 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-02507631a9f7bc956) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0e7f661f69bb5d6b4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0e7f661f69bb5d6b4) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-00e0090ac21971297 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-00e0090ac21971297) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-01cb1066e1ad93cba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-01cb1066e1ad93cba) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-052f2fa11c7145e04 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-052f2fa11c7145e04) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0400d18ee6d078a95 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0400d18ee6d078a95) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-05af3f57a0b59fb78 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-05af3f57a0b59fb78) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0e8baaccc62ee0a9f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0e8baaccc62ee0a9f) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-01711df8fe87a6217 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-01711df8fe87a6217) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-04fc06e24a65297fb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-04fc06e24a65297fb) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-09577c19fbe1bd7fa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-09577c19fbe1bd7fa) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0c788f17fd2f1f650 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0c788f17fd2f1f650) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-04a084a6d17d9816e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-04a084a6d17d9816e) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-013b322dbc79e9a6a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-013b322dbc79e9a6a) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-071f4e4006f9c3211 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-071f4e4006f9c3211) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-01569d819ef2d5743 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-01569d819ef2d5743) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0d6839e319ca398b9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0d6839e319ca398b9) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-f6b1ca97 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-f6b1ca97) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.29.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-04d57166b412182ca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-04d57166b412182ca) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-054c7523f88819252 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-054c7523f88819252) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-06fe7e1b2c2e1c403 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-06fe7e1b2c2e1c403) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-0d2bc2ef86b794322 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0d2bc2ef86b794322) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-03c7481fbd861a309 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-03c7481fbd861a309) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-0e080da108f456eda | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0e080da108f456eda) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-0e4f39dd120f55ab2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0e4f39dd120f55ab2) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190617\-arm64\-ebs | ami\-0fe0d679d79cb2562 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0fe0d679d79cb2562) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.29.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.0-1`\)
+ The recommended NVIDIA driver version \(`418.40.04`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`9.2.88`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-02bb50f6d2a052856 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-02bb50f6d2a052856) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0d334c42a27f3518a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0d334c42a27f3518a) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-081c9eb1157e6df56 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-081c9eb1157e6df56) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-07aecceb474f62374 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-07aecceb474f62374) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-09b91ad3d83dfc37e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-09b91ad3d83dfc37e) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0c1eb6a63ac3b9bb1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0c1eb6a63ac3b9bb1) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0300aabae97c41597 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0300aabae97c41597) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-07fac902ed2f0fc4f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-07fac902ed2f0fc4f) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-097afb35fa0bbda8f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-097afb35fa0bbda8f) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-087dff31c28befb87 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-087dff31c28befb87) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0a3f00e10b0b01fd6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0a3f00e10b0b01fd6) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-04a9fd81a4eb30837 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-04a9fd81a4eb30837) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0c48369105778c21a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0c48369105778c21a) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0fa8b2586dc0d989e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0fa8b2586dc0d989e) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0d5ad99ef46857019 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0d5ad99ef46857019) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-065a0ac9ffdf045ce | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-065a0ac9ffdf045ce) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-0d8c20b2ec35b644d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0d8c20b2ec35b644d) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-05470a522ce6d3be7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-05470a522ce6d3be7) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190614\-x86\_64\-ebs | ami\-93bcc7f2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-93bcc7f2) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.29.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0eba5aab4550a443a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0eba5aab4550a443a) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0d09143c6fc181fe3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0d09143c6fc181fe3) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-00303cd65a37d033b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-00303cd65a37d033b) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-084799b9fb64c149e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-084799b9fb64c149e) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-053d6ef319599211f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-053d6ef319599211f) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0e1aa8c2e9d719f58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e1aa8c2e9d719f58) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0765a9b4036f26f32 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0765a9b4036f26f32) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-09155a5dc3c532de1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-09155a5dc3c532de1) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0309369aa9694281c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0309369aa9694281c) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0c2963a1e04bd8a00 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0c2963a1e04bd8a00) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-00d5c6d1f8349edd1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-00d5c6d1f8349edd1) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0c0c01a7a42f41c0c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0c0c01a7a42f41c0c) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-01d37f0c18610fdc6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-01d37f0c18610fdc6) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-03d739ab8755ab020 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-03d739ab8755ab020) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0959f069afa43696a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0959f069afa43696a) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-025da470c90f48af8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-025da470c90f48af8) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-01c4f4ee99cf38e79 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-01c4f4ee99cf38e79) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-0f85cbfbcffb10b08 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0f85cbfbcffb10b08) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.u\-amazon\-ecs\-optimized | ami\-91b1caf0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-91b1caf0) | 

------
#### [ Windows Server 2019 ]

The current Amazon ECS\-optimized Windows 2019 AMI consists of:
+ The latest version of Microsoft Windows Server 2019
+ Docker EE version `18.09.4`
+ Amazon ECS container agent version `1.27.0`

The following table lists the current Amazon ECS\-optimized Windows 2019 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0a4548e9bef884a63 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0a4548e9bef884a63) | 
| us\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0f7cc2a4e9cb93130 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0f7cc2a4e9cb93130) | 
| us\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0caa9f58a76b75d76 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0caa9f58a76b75d76) | 
| us\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-09a6b4fc9786621ef | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-09a6b4fc9786621ef) | 
| ap\-northeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0de9f680eb139f5f2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0de9f680eb139f5f2) | 
| ap\-northeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-052dc171cf22efb2c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-052dc171cf22efb2c) | 
| ap\-south\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0fda456670ecdda47 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0fda456670ecdda47) | 
| ap\-southeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0d073901cb231d495 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0d073901cb231d495) | 
| ap\-southeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-06cef3b9805e5ebb0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-06cef3b9805e5ebb0) | 
| ca\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-09f37f76841876c2b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-09f37f76841876c2b) | 
| cn\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-02942c66816678482 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-02942c66816678482) | 
| cn\-northwest\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-066eff0f2473d2ba3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-066eff0f2473d2ba3) | 
| eu\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-09bff64c8c3102238 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-09bff64c8c3102238) | 
| eu\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-078d39ec1c8b11d6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-078d39ec1c8b11d6b) | 
| eu\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-05da69b2d804943e6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-05da69b2d804943e6) | 
| eu\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-01fbd6d84ec8b36d3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-01fbd6d84ec8b36d3) | 
| eu\-west\-3 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0a64405322f93a0c7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0a64405322f93a0c7) | 
| sa\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0ac8048de25ce4284 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0ac8048de25ce4284) | 
| us\-gov\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-0c4c54ea7fe80d45a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0c4c54ea7fe80d45a) | 
| us\-gov\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10 | ami\-d91f63b8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-d91f63b8) | 

------
#### [ Windows Server 2016 ]

The current Amazon ECS\-optimized Windows 2016 AMI consists of:
+ The latest version of Microsoft Windows Server 2016
+ Docker EE version `18.03.1-ee-7`
+ Amazon ECS container agent version `1.26.0`

The following table lists the current Amazon ECS\-optimized Windows 2016 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-00c56e74f090d6f65 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-00c56e74f090d6f65) | 
| us\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0ed2f29599018e745 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0ed2f29599018e745) | 
| us\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-01b55f7fe967f727b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-01b55f7fe967f727b) | 
| us\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-07f6b98dc6c8067c3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-07f6b98dc6c8067c3) | 
| ap\-northeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-07839df9eec55ac8d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-07839df9eec55ac8d) | 
| ap\-northeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-071b78467d9d35580 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-071b78467d9d35580) | 
| ap\-south\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0183732d8e0fd56c7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0183732d8e0fd56c7) | 
| ap\-southeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0a6c13d83c0fdbf2b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0a6c13d83c0fdbf2b) | 
| ap\-southeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-06d33f81ca8384556 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-06d33f81ca8384556) | 
| ca\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-098ad73a3005be676 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-098ad73a3005be676) | 
| cn\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0b484446add9a27b3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-0b484446add9a27b3) | 
| cn\-northwest\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-047cc7df873d123f2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-047cc7df873d123f2) | 
| eu\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0f7386282aa13a0d8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0f7386282aa13a0d8) | 
| eu\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-06e3cb4d2875b172e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-06e3cb4d2875b172e) | 
| eu\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-04a2fa8ce0fc20c61 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-04a2fa8ce0fc20c61) | 
| eu\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0fac4f3bdab9ccddc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0fac4f3bdab9ccddc) | 
| eu\-west\-3 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-06a5b6fc522511993 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-06a5b6fc522511993) | 
| sa\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-05889298c47e6d5c2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-05889298c47e6d5c2) | 
| us\-gov\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-0b6f703732ae49d69 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0b6f703732ae49d69) | 
| us\-gov\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07 | ami\-9d91fafc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-9d91fafc) | 

------

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.