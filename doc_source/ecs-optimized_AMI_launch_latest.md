# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 Console Link** in one of the tables below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized AMI ID, according to the variant you choose, below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

------
#### [ Amazon Linux 2 ]

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.28.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-012ca23958772cf72 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-012ca23958772cf72) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-00cf4737e238866a3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-00cf4737e238866a3) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-06d87f0156b1d4407 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-06d87f0156b1d4407) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0a9f5be2a016dccad | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0a9f5be2a016dccad) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-067f4f7124e746edd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-067f4f7124e746edd) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0e52aad6ac7733a6a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e52aad6ac7733a6a) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-08834c8c57e502d6d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-08834c8c57e502d6d) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-04322e867758d97a8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-04322e867758d97a8) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0047bfdb16f1f6781 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0047bfdb16f1f6781) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-09475847322e5566f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-09475847322e5566f) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0498c464ec4d2ba83 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0498c464ec4d2ba83) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-096a38c97b80cd8ec | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-096a38c97b80cd8ec) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0dddc4daca44e6e99 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0dddc4daca44e6e99) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0c5abd45f676aab4f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0c5abd45f676aab4f) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0204aa6a92a54561e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0204aa6a92a54561e) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-07273195833e4f20c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-07273195833e4f20c) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-00d851648873aaabc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-00d851648873aaabc) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-06473be43b0f77600 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-06473be43b0f77600) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-607c0001 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-607c0001) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.28.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-arm64\-ebs | ami\-030392040d1aed930 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-030392040d1aed930) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-arm64\-ebs | ami\-0e7126260e3c3f9b0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0e7126260e3c3f9b0) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-arm64\-ebs | ami\-037a4247c72ff5782 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-037a4247c72ff5782) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190510\-arm64\-ebs | ami\-04d7703e789babb4a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-04d7703e789babb4a) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.28.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.0-1`\)
+ The recommended NVIDIA driver version \(`418.40.04`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`9.2.88`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0f132b270b9aabeca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0f132b270b9aabeca) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0b0ae551a867891da | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0b0ae551a867891da) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0ca127ab2bfadf65d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0ca127ab2bfadf65d) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-083612cfef21db11d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-083612cfef21db11d) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0ce92ebe7a58225a8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0ce92ebe7a58225a8) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0a090420c9bdbd47f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0a090420c9bdbd47f) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0aa98500408657f87 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0aa98500408657f87) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-06eec8c54f8a637e9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-06eec8c54f8a637e9) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-03893996ba1620bc0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-03893996ba1620bc0) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-01d49902dbdd24819 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-01d49902dbdd24819) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-00136313cb23e1bcd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-00136313cb23e1bcd) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0eda13fcf629f5f0c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-0eda13fcf629f5f0c) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0a004e87a7189cdf3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0a004e87a7189cdf3) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0123f684a7258c751 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0123f684a7258c751) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-051ff72fce0a8bafe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-051ff72fce0a8bafe) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0ba990e211024cbff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0ba990e211024cbff) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-0bae1ac39b3ab1c25 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0bae1ac39b3ab1c25) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-06d9106a2e6079a8b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-06d9106a2e6079a8b) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-01eec887a06c2d4bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-01eec887a06c2d4bf) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-05dcc99800a097fb5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-05dcc99800a097fb5) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190510\-x86\_64\-ebs | ami\-bd7b07dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-bd7b07dc) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.28.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-06a8ae0ecd30e804c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-06a8ae0ecd30e804c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0750ab1027b6314c7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0750ab1027b6314c7) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-03fe84be94ca9cc17 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-03fe84be94ca9cc17) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-043c4e6bff652b99e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-043c4e6bff652b99e) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0d772c70a2d689e8b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0d772c70a2d689e8b) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-062ef2a2561c9364a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-062ef2a2561c9364a) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0005600074f3aa4be | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0005600074f3aa4be) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0a80c5ae873c08c64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0a80c5ae873c08c64) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0b78efd7fafc3f93a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0b78efd7fafc3f93a) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0c5058003c511da15 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0c5058003c511da15) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0005ff694f167b58a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0005ff694f167b58a) | 
| cn\-north\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0005ce40ccef58b98 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-0005ce40ccef58b98) | 
| cn\-northwest\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0066a513748afa1e0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0066a513748afa1e0) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-06a20f16dd2f50741 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-06a20f16dd2f50741) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-087de2c1b54c6bd93 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-087de2c1b54c6bd93) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-09b156894255325fe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-09b156894255325fe) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-03ca259ae4cb86837 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-03ca259ae4cb86837) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-055c29a7d5fc2d4a8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-055c29a7d5fc2d4a8) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0d83f147ba8afa3cf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0d83f147ba8afa3cf) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-0b93f4db7ff03a1b1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0b93f4db7ff03a1b1) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.q\-amazon\-ecs\-optimized | ami\-e97a0688 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-e97a0688) | 

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