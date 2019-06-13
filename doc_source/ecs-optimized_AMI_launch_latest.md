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
+ The latest version of the Amazon ECS container agent \(`1.29.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.29.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-068a784e5da70c400 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-068a784e5da70c400) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0dde61416371df99a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0dde61416371df99a) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-01b403cf431fecff5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-01b403cf431fecff5) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-050274527f8727b52 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-050274527f8727b52) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0daae7f52f79d626d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0daae7f52f79d626d) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-088d9a38123ee2d21 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-088d9a38123ee2d21) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-009ef65c02eb7db10 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-009ef65c02eb7db10) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-03f59e90c8855694e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-03f59e90c8855694e) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0c8ef5b3cf8888930 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0c8ef5b3cf8888930) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-096d467b43b2344ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-096d467b43b2344ba) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-079442f710b115509 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-079442f710b115509) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-075703041f2f591b9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-075703041f2f591b9) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-07fcc859b2567e0c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-07fcc859b2567e0c4) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0f43fe59461776205 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0f43fe59461776205) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0294bb049c608a183 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0294bb049c608a183) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-065d86bd1f3350c52 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-065d86bd1f3350c52) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-024b28d14f975baf4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-024b28d14f975baf4) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-04f386acc5a9ab7d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-04f386acc5a9ab7d1) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-9ebec6ff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-9ebec6ff) | 

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
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-02ff5a80684cb5988 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-02ff5a80684cb5988) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-0cf21904bbdc31e86 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0cf21904bbdc31e86) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-0e7cf2fc7390e94ce | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0e7cf2fc7390e94ce) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-018b1cae456af0c1a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-018b1cae456af0c1a) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-072db467449a11969 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-072db467449a11969) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs |  | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-00a7685ac12bdcd1a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-00a7685ac12bdcd1a) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190607\-arm64\-ebs | ami\-0947130aa57e453f1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0947130aa57e453f1) | 

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
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0e090ceefe711ac95 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0e090ceefe711ac95) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-01f1db40c7cbd1a37 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-01f1db40c7cbd1a37) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-00951175e0c3d094f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-00951175e0c3d094f) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0a4976e71664d2be7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0a4976e71664d2be7) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0daae7f52f79d626d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0daae7f52f79d626d) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-09171da22d9de354c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-09171da22d9de354c) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-078283c1b9c4f352d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-078283c1b9c4f352d) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0670058400a934447 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0670058400a934447) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-01a52575d6cb3e30e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-01a52575d6cb3e30e) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0e874f2ef4a4be5f7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0e874f2ef4a4be5f7) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0ef3e87b9caee0a7a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0ef3e87b9caee0a7a) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-06dd029a229aed1cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-06dd029a229aed1cb) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-080e40f6256efa727 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-080e40f6256efa727) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0b3c6e06179ecf433 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0b3c6e06179ecf433) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-03c204616192e8913 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-03c204616192e8913) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-00ab7019317c097ff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-00ab7019317c097ff) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0cc1191b5880fab81 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0cc1191b5880fab81) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-093882618166e2c74 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-093882618166e2c74) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0058717cd5d9fea81 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0058717cd5d9fea81) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-0dabf3cef9b0e0901 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0dabf3cef9b0e0901) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190607\-x86\_64\-ebs | ami\-fabfc79b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-fabfc79b) | 

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
| us\-east\-2 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0c8af9d51bfa2dbc0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0c8af9d51bfa2dbc0) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-036cea62390485c0b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-036cea62390485c0b) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0b9b1c881e7d2a6e2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0b9b1c881e7d2a6e2) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-07882cc549408d6ab | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-07882cc549408d6ab) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0e518e01372f998ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0e518e01372f998ba) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0e4ea2004b1254071 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e4ea2004b1254071) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0d70c328a23036109 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0d70c328a23036109) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-021cade0f478e2285 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-021cade0f478e2285) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0450397bd25f3d552 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0450397bd25f3d552) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0a22ed784dba43529 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0a22ed784dba43529) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0380caea3b11e78a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0380caea3b11e78a2) | 
| cn\-north\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-065f43418e55f35d6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-065f43418e55f35d6) | 
| cn\-northwest\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-016c7365746cfaf17 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-016c7365746cfaf17) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-03033f455185c4b8a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-03033f455185c4b8a) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0a66775349a33afe5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0a66775349a33afe5) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-08678d6fe34d95154 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-08678d6fe34d95154) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-0582914fe1d0dfd75 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0582914fe1d0dfd75) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-07bc0141838438a38 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-07bc0141838438a38) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-08d9602dcef4c6cb1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-08d9602dcef4c6cb1) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-069f2a1ee92877b7c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-069f2a1ee92877b7c) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.t\-amazon\-ecs\-optimized | ami\-babac2db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-babac2db) | 

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