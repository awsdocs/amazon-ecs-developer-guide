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
+ The latest version of the Amazon ECS container agent \(`1.29.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.1-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0dca97e7cde7be3d5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0dca97e7cde7be3d5) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0fac5486e4cff37f4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0fac5486e4cff37f4) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0c6e63b58aac1048e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0c6e63b58aac1048e) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0e5e051fd0b505db6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0e5e051fd0b505db6) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-02252d984c7e3595d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-02252d984c7e3595d) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-04a735b489d2a0320 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-04a735b489d2a0320) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0accbb5aa909be7bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0accbb5aa909be7bf) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0a8bf4e187339e2c1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0a8bf4e187339e2c1) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-05c6d22d98f97471c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-05c6d22d98f97471c) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-039bb4c3a7946ce19 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-039bb4c3a7946ce19) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-01c07ee95e77abba8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-01c07ee95e77abba8) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0650e7d86452db33b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0650e7d86452db33b) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-059aa04f0c253ad6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-059aa04f0c253ad6b) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0ae254c8a2d3346a7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0ae254c8a2d3346a7) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0de1dc478496a9e9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0de1dc478496a9e9b) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0d260f3e5ccd06043 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0d260f3e5ccd06043) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-038707d64e5b8e7ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-038707d64e5b8e7ba) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0a224902b35f8ad6c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0a224902b35f8ad6c) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-04c68165 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-04c68165) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0b619dec66f0fe1eb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-0b619dec66f0fe1eb) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0b619dec66f0fe1eb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0b619dec66f0fe1eb) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.29.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.1-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-arm64\-ebs | ami\-02a59fb754c85ab16 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-02a59fb754c85ab16) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-arm64\-ebs | ami\-0b3b892651e52f03d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0b3b892651e52f03d) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-arm64\-ebs | ami\-0860edcdc9c9533e3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0860edcdc9c9533e3) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190709\-arm64\-ebs | ami\-0af65bdd9a59a3171 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0af65bdd9a59a3171) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.29.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.1-1`\)
+ The recommended NVIDIA driver version \(`418.40.04`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`9.2.88`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0522cd69e8a331c8a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0522cd69e8a331c8a) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0875a1e6c2db1cdc9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0875a1e6c2db1cdc9) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0dfe31c7ed6577ebf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0dfe31c7ed6577ebf) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0bf12263037af5756 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0bf12263037af5756) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0efcda0accbf5a0f4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0efcda0accbf5a0f4) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-041843b72dde36df4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-041843b72dde36df4) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0e18b01ac7608adcd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0e18b01ac7608adcd) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0c5cdf93d467beca8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0c5cdf93d467beca8) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-07b25dd477ea57f82 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-07b25dd477ea57f82) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-07f990e4e7551b774 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-07f990e4e7551b774) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-08e1aab6a2cfd879d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-08e1aab6a2cfd879d) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-096ccf1edd625875e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-096ccf1edd625875e) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0576489203c1c4ccc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0576489203c1c4ccc) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-03867df65b0e5ab52 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-03867df65b0e5ab52) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0faecf24ba37edfce | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0faecf24ba37edfce) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0dec5973b167d5ff3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0dec5973b167d5ff3) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0a398510537094972 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0a398510537094972) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0365a6c905c4ba391 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0365a6c905c4ba391) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-f8b8ff99 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-f8b8ff99) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-01143a340285bc1fb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-01143a340285bc1fb) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190709\-x86\_64\-ebs | ami\-0240ef2b16197fef3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0240ef2b16197fef3) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.29.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.1-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0151b45908571e14c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0151b45908571e14c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0c09d65d2051ada93 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0c09d65d2051ada93) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0b4f28359911d5896 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0b4f28359911d5896) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-077368b501184adb9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-077368b501184adb9) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0eeaf74d330e7e8ca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0eeaf74d330e7e8ca) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-084d4626fec6f3ca3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-084d4626fec6f3ca3) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-03c5d1f1ba6ec1b7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-03c5d1f1ba6ec1b7d) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-09f6bc049f323302c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-09f6bc049f323302c) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-01813416d767d88db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-01813416d767d88db) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-07fcafbb9f3a4f7db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-07fcafbb9f3a4f7db) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-011742239f8f42f46 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-011742239f8f42f46) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0bceb1887b6b37130 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0bceb1887b6b37130) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0d7012b0cae33c045 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0d7012b0cae33c045) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-02b9d5ef54d57fb7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-02b9d5ef54d57fb7d) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0f49b2a9014635082 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0f49b2a9014635082) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0db8d6e7dcf3cb362 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0db8d6e7dcf3cb362) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0ad0be326813dda96 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0ad0be326813dda96) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-033b28d8ef5fe3f73 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-033b28d8ef5fe3f73) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-b0bafdd1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-b0bafdd1) | 
| cn\-north\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-041f913b515304af9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-041f913b515304af9) | 
| cn\-northwest\-1 | amzn\-ami\-2018\.03\.v\-amazon\-ecs\-optimized | ami\-0be42f3b41b17b513 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0be42f3b41b17b513) | 

------
#### [ Windows Server 2019 ]

The current Amazon ECS\-optimized Windows 2019 AMI consists of:
+ The latest version of Microsoft Windows Server 2019
+ Docker EE version `18.09.8`
+ Amazon ECS container agent version `1.29.0`

The following table lists the current Amazon ECS\-optimized Windows 2019 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-05c650b3e1759298f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-05c650b3e1759298f) | 
| us\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-00f3f16f1d05e53a9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-00f3f16f1d05e53a9) | 
| us\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-08f56c127cc9ae9c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-08f56c127cc9ae9c4) | 
| us\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0c37f25e59d752c61 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0c37f25e59d752c61) | 
| ap\-northeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0771bfb73bc362295 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0771bfb73bc362295) | 
| ap\-northeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0849ba95174008d74 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0849ba95174008d74) | 
| ap\-south\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0dfe1bb9ed2dd2ff1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0dfe1bb9ed2dd2ff1) | 
| ap\-southeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-06daa78e84b800914 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-06daa78e84b800914) | 
| ap\-southeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-071e43d09ba6f2479 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-071e43d09ba6f2479) | 
| ca\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0ed093f8cd0bb6f64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0ed093f8cd0bb6f64) | 
| eu\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-020bded18f4df2c7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-020bded18f4df2c7d) | 
| eu\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0269f4d7fa129ddcb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0269f4d7fa129ddcb) | 
| eu\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0e3604f46f9ccc506 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0e3604f46f9ccc506) | 
| eu\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0746c037f8889f6e8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0746c037f8889f6e8) | 
| eu\-west\-3 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-022ae3cb06a066371 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-022ae3cb06a066371) | 
| sa\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-094ff1452e5849bff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-094ff1452e5849bff) | 
| us\-gov\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0e238cac0ef640949 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0e238cac0ef640949) | 
| us\-gov\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-13fcbd72 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-13fcbd72) | 
| cn\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-02728cc741b470390 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-02728cc741b470390) | 
| cn\-northwest\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0e1cafc0e9ae3faf8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0e1cafc0e9ae3faf8) | 

------
#### [ Windows Server 2016 ]

The current Amazon ECS\-optimized Windows 2016 AMI consists of:
+ The latest version of Microsoft Windows Server 2016
+ Docker EE version `18.09.8`
+ Amazon ECS container agent version `1.29.0`

The following table lists the current Amazon ECS\-optimized Windows 2016 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0ca3903548e4a8681 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0ca3903548e4a8681) | 
| us\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0831d21a034cb33e4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0831d21a034cb33e4) | 
| us\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-08b04db86cc171254 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-08b04db86cc171254) | 
| us\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0b380fdc264da2fc3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0b380fdc264da2fc3) | 
| ap\-northeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0d3f659d5bab5c8e5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0d3f659d5bab5c8e5) | 
| ap\-northeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0d4fe34a4a5cb98ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0d4fe34a4a5cb98ba) | 
| ap\-south\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0add8558b7afe4960 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0add8558b7afe4960) | 
| ap\-southeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-001b756da3bc2770c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-001b756da3bc2770c) | 
| ap\-southeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-063f16052f2b588a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-063f16052f2b588a2) | 
| ca\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-07cc1c48c86d988ca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-07cc1c48c86d988ca) | 
| eu\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0644eb70b836aa55a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0644eb70b836aa55a) | 
| eu\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-038a1a6d22e486228 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-038a1a6d22e486228) | 
| eu\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0e41be650397b131d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0e41be650397b131d) | 
| eu\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0884e795ec6511040 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0884e795ec6511040) | 
| eu\-west\-3 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-03f921d23a15ff701 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-03f921d23a15ff701) | 
| sa\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-06955dc02fc2a3b7b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-06955dc02fc2a3b7b) | 
| us\-gov\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-04c0f4fe213678f9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-04c0f4fe213678f9b) | 
| us\-gov\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-38fdbc59 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-38fdbc59) | 
| cn\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-010da5256208bcb66 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-010da5256208bcb66) | 
| cn\-northwest\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19 | ami\-0c1033d4d46070747 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0c1033d4d46070747) | 

------

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.