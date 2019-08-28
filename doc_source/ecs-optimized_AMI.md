# Amazon ECS\-optimized AMIs<a name="ecs-optimized_AMI"></a>

The basic Amazon Elastic Container Service \(Amazon ECS\) container instance specification consists of the following:

**Required**
+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.
+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.
+ A Docker daemon running at least version 1\.9\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS agent version that you are using\. For more information, see [Amazon ECS Container Agent Versions](container_agent_versions.md)\.

**Recommended**
+ An initialization and nanny process to run and monitor the Amazon ECS agent\. The Amazon ECS\-optimized AMIs use the `ecs-init` RPM to manage the agent\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

The Amazon ECS\-optimized AMIs are preconfigured with these requirements and recommendations\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your container instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Amazon ECS vends AMIs that are optimized for the service in the following variants\.
+ **Amazon ECS\-optimized Amazon Linux 2 AMI** – Recommended for launching your Amazon ECS container instances in most cases\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI** – Recommended for launching your Amazon ECS container instances when using the Amazon EC2 A1 instance type, which is powered by Arm\-based AWS Graviton Processors\. For more information, see [General Purpose Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ **Amazon ECS GPU\-optimized AMI** – Recommended for launching your Amazon ECS container instances when working with GPU workloads\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.
+ **Amazon ECS\-optimized Amazon Linux AMI** – This AMI is based off of Amazon Linux 1\. We recommend that you migrate your workloads to the Amazon ECS\-optimized Amazon Linux 2 AMI\. Support for the Amazon ECS\-optimized Amazon Linux AMI ends no later than June 30, 2020\.
+ **Amazon ECS\-optimized Windows 2019 AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows 2016 AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.

Although you can create your own container instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

View the AMI IDs on one of the following tabs, according to the variant you choose\.

------
#### [ Amazon Linux 2 ]

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.30.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.30.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0e7c12c1bedd6bf21 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0e7c12c1bedd6bf21) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0b16d80945b1a9c7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0b16d80945b1a9c7d) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-08a12265d9e050d57 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-08a12265d9e050d57) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0e434a58221275ed4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0e434a58221275ed4) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-05bfc466e50bdfb65 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-05bfc466e50bdfb65) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0e37e42dff65024ae | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e37e42dff65024ae) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-000fbda700ba8fe9d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-000fbda700ba8fe9d) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0b7f1d57770573a75 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0b7f1d57770573a75) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-05357ea4dad5e2cf4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-05357ea4dad5e2cf4) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0c7dea114481e059d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0c7dea114481e059d) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-052de4ecee980719c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-052de4ecee980719c) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0e1d30823ff9f8459 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0e1d30823ff9f8459) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-063b45c1e31fdc5d2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-063b45c1e31fdc5d2) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-02bf9e90a6e30dc74 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-02bf9e90a6e30dc74) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-010624faf51b049d3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-010624faf51b049d3) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-084e49fa6ca9c8794 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-084e49fa6ca9c8794) | 
| me\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0375ec9d8a8bcf612 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=me-south-1#LaunchInstanceWizard:ami=ami-0375ec9d8a8bcf612) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-051d98f19acea0389 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-051d98f19acea0389) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-083cf4d1d5ca46c23 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-083cf4d1d5ca46c23) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-8d511eec | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-8d511eec) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-03e4887abba02fadb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-03e4887abba02fadb) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-00a7b74b1f028cf0d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-00a7b74b1f028cf0d) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.30.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.30.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-arm64\-ebs | ami\-036f6ddd491fd6009 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-036f6ddd491fd6009) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-arm64\-ebs | ami\-01a325c826e291b64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-01a325c826e291b64) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-arm64\-ebs | ami\-070f0c8512914277a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-070f0c8512914277a) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190815\-arm64\-ebs | ami\-07e41fbf04f9e0d6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-07e41fbf04f9e0d6b) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.30.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.30.0-1`\)
+ The recommended NVIDIA driver version \(`418.40.04`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`9.2.88`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0d9a105d85d46ce21 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0d9a105d85d46ce21) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0180e79579e32b7e6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0180e79579e32b7e6) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0f2d4e4a159b148a9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0f2d4e4a159b148a9) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0a795f51aebcbe0c1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0a795f51aebcbe0c1) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0a1476f135fcd55c8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0a1476f135fcd55c8) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0b347740f06f9dd72 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0b347740f06f9dd72) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-093f2e63de5d4b480 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-093f2e63de5d4b480) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-06b37b182941ed228 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-06b37b182941ed228) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-08d4d1bc5d79e56cf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-08d4d1bc5d79e56cf) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-005ff23ea5572250c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-005ff23ea5572250c) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0b975c72ed95864d8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0b975c72ed95864d8) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-08430f68d6875a40a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-08430f68d6875a40a) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0f844d07a837de7e8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0f844d07a837de7e8) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0a5532e0793a984d9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0a5532e0793a984d9) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-016ba03f07656e55b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-016ba03f07656e55b) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-09d347753b8ee2d64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-09d347753b8ee2d64) | 
| me\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-07262aec553b6aa79 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=me-south-1#LaunchInstanceWizard:ami=ami-07262aec553b6aa79) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0aa505607f459a952 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0aa505607f459a952) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0ce1af554c76dfa4f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0ce1af554c76dfa4f) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-05511e64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-05511e64) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-0645a2a7685225a80 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-0645a2a7685225a80) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190815\-x86\_64\-ebs | ami\-01e13577489b67d12 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-01e13577489b67d12) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.30.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.30.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-035a1bdaf0e4bf265 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-035a1bdaf0e4bf265) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-066ce9bb9f4cbb03d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-066ce9bb9f4cbb03d) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0413317a44231a219 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0413317a44231a219) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0fd6e28e415664140 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0fd6e28e415664140) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0bc9197107aa2e36d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-0bc9197107aa2e36d) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0a4b5b999281c955b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0a4b5b999281c955b) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0c6e04cbad6e3e13f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0c6e04cbad6e3e13f) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-04a56cb1908b6ce94 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-04a56cb1908b6ce94) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-003cb73efe1eb03cc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-003cb73efe1eb03cc) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0112bb4988eedc594 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0112bb4988eedc594) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-02a835991f91d92cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-02a835991f91d92cb) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0b0a910db6581d75f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0b0a910db6581d75f) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-03147f238a3100ad7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-03147f238a3100ad7) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0d2aaec13a6b7e7ca | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0d2aaec13a6b7e7ca) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0e2d2ad19e82df43b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e2d2ad19e82df43b) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0244cc5d7b6e6d95e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0244cc5d7b6e6d95e) | 
| me\-south\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-06af5a12626c763dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=me-south-1#LaunchInstanceWizard:ami=ami-06af5a12626c763dc) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-0149bb9bc7a19cf0e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0149bb9bc7a19cf0e) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-09c03f1d4c50330f7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-09c03f1d4c50330f7) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-ac521dcd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-ac521dcd) | 
| cn\-north\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-099eefd6de9767dd1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-099eefd6de9767dd1) | 
| cn\-northwest\-1 | amzn\-ami\-2018\.03\.w\-amazon\-ecs\-optimized | ami\-009c89a26befaa039 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-009c89a26befaa039) | 

------
#### [ Windows Server 2019 ]

The current Amazon ECS\-optimized Windows 2019 AMI consists of:
+ The latest version of Microsoft Windows Server 2019
+ Docker EE version `19.03.1`
+ Amazon ECS container agent version `1.29.1`

The following table lists the current Amazon ECS\-optimized Windows 2019 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-02e4adf45a639642c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-02e4adf45a639642c) | 
| us\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-088e9fda5bbd0f15e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-088e9fda5bbd0f15e) | 
| us\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0d200ec2e8b96a8a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0d200ec2e8b96a8a2) | 
| us\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0f104ab6be55ae8ff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0f104ab6be55ae8ff) | 
| ap\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-00407e0b636b1a190 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-00407e0b636b1a190) | 
| ap\-northeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-00dc3554018a145d4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-00dc3554018a145d4) | 
| ap\-northeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0f92d045353fdf71b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0f92d045353fdf71b) | 
| ap\-south\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0e5479b8c125fbf50 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0e5479b8c125fbf50) | 
| ap\-southeast\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-00012286a4d6116e9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-00012286a4d6116e9) | 
| ap\-southeast\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-086a3862259cdf912 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-086a3862259cdf912) | 
| ca\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0b7511e837f93de9a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0b7511e837f93de9a) | 
| eu\-central\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-08168a40cb0a7181c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-08168a40cb0a7181c) | 
| eu\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0e4c6f2b6b20dfe80 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0e4c6f2b6b20dfe80) | 
| eu\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-08df59ca9269bb243 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-08df59ca9269bb243) | 
| eu\-west\-2 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-03d073ae685dbaa20 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-03d073ae685dbaa20) | 
| eu\-west\-3 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0ee955047602d7485 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0ee955047602d7485) | 
| me\-south\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0e0cc32afc8b6f5bd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=me-south-1#LaunchInstanceWizard:ami=ami-0e0cc32afc8b6f5bd) | 
| sa\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0130df2f7d7c85e0d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0130df2f7d7c85e0d) | 
| us\-gov\-east\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0a83bb541012d1b87 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0a83bb541012d1b87) | 
| us\-gov\-west\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-5ab4fb3b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-5ab4fb3b) | 
| cn\-north\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-03260f74d63fab3f1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-03260f74d63fab3f1) | 
| cn\-northwest\-1 | Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0b17a49998dc4f1ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0b17a49998dc4f1ba) | 

------
#### [ Windows Server 2016 ]

The current Amazon ECS\-optimized Windows 2016 AMI consists of:
+ The latest version of Microsoft Windows Server 2016
+ Docker EE version `19.03.1`
+ Amazon ECS container agent version `1.29.1`

The following table lists the current Amazon ECS\-optimized Windows 2016 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0752dc6505f07210d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0752dc6505f07210d) | 
| us\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-06b130a3302402f46 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-06b130a3302402f46) | 
| us\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0e65dca6ee7c0b773 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0e65dca6ee7c0b773) | 
| us\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0bcca0865e651a4ed | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0bcca0865e651a4ed) | 
| ap\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-022de1860ce4897ae | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-022de1860ce4897ae) | 
| ap\-northeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0ee598cc9eb68571b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0ee598cc9eb68571b) | 
| ap\-northeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-009e028c4df12dc71 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-009e028c4df12dc71) | 
| ap\-south\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-018b395a4b46433de | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-018b395a4b46433de) | 
| ap\-southeast\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0a645f18b0bf686f6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0a645f18b0bf686f6) | 
| ap\-southeast\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-057499595fae5ab87 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-057499595fae5ab87) | 
| ca\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-098f793b06fb99431 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-098f793b06fb99431) | 
| eu\-central\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-05187a19c644ff317 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-05187a19c644ff317) | 
| eu\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-00c957867ffa2bc3b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-00c957867ffa2bc3b) | 
| eu\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-060ad53d54da5534c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-060ad53d54da5534c) | 
| eu\-west\-2 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-078e72e05ac8cd6da | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-078e72e05ac8cd6da) | 
| eu\-west\-3 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0acc8b265e96b56e5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0acc8b265e96b56e5) | 
| me\-south\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-0a6b6ab63e476d685 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=me-south-1#LaunchInstanceWizard:ami=ami-0a6b6ab63e476d685) | 
| sa\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-01869ba264370f356 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-01869ba264370f356) | 
| us\-gov\-east\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-01f03a6726390ce22 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-01f03a6726390ce22) | 
| us\-gov\-west\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-f2b2fd93 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-f2b2fd93) | 
| cn\-north\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-077f72816723f914a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-077f72816723f914a) | 
| cn\-northwest\-1 | Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16 | ami\-04369da83d59820a0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-04369da83d59820a0) | 

------

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)
+ [AMI Storage Configuration](ecs-ami-storage-config.md)