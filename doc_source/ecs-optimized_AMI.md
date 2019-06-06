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
+ The latest version of the Amazon ECS container agent \(`1.28.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.1-2`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0c41b421bf4efab32 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0c41b421bf4efab32) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0f812849f5bc97db5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0f812849f5bc97db5) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-09927f2913ee48e79 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-09927f2913ee48e79) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-096cf539543dfbe40 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-096cf539543dfbe40) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-061b08c2b98d7360c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-061b08c2b98d7360c) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0d567487508d0833c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0d567487508d0833c) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0ae430e764245a879 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0ae430e764245a879) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-07083d1dc5c33cb83 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-07083d1dc5c33cb83) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-00f596fcbda0cebcc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-00f596fcbda0cebcc) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0d68223af95689f61 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0d68223af95689f61) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0ff993c9d53f2b53c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0ff993c9d53f2b53c) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0fbac225f3afdf632 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0fbac225f3afdf632) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-02c28719fc27120c1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-02c28719fc27120c1) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0d9430336a60e81c5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0d9430336a60e81c5) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0bcdb1fbd79cc1a6f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0bcdb1fbd79cc1a6f) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0f2070b01db3ba354 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0f2070b01db3ba354) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0751caf32a4ba7a5e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0751caf32a4ba7a5e) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0823cd1a7c931a0e3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0823cd1a7c931a0e3) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-88cdb4e9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-88cdb4e9) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.28.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.1-2`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-03c38ea721225c014 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-03c38ea721225c014) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-04d28a845dc4d52dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-04d28a845dc4d52dc) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-001d8b3d9aa726a4e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-001d8b3d9aa726a4e) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-01bfff51a2b40ab12 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-01bfff51a2b40ab12) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-0c16f173b694c17c6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0c16f173b694c17c6) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-060fce4261e22196c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-060fce4261e22196c) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-0156ae3ae679e62cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0156ae3ae679e62cb) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190603\-arm64\-ebs | ami\-0b2a593900555253e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0b2a593900555253e) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.28.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.1-2`\)
+ The recommended NVIDIA driver version \(`418.40.04`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`9.2.88`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-04b4d362520459a1d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-04b4d362520459a1d) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-071a2902c1f3e36ea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-071a2902c1f3e36ea) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0344ed59029a345a8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0344ed59029a345a8) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-043160836afc9dfa9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-043160836afc9dfa9) | 
| ap\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-09a4bc852df9e019b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-09a4bc852df9e019b) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-05368f1bd0c7d99a9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-05368f1bd0c7d99a9) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-049eeb410d5288690 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-049eeb410d5288690) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0eba844a3a01f7e0e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0eba844a3a01f7e0e) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0975a2237f616abc8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0975a2237f616abc8) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0ac32de2e3ff0ac9d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0ac32de2e3ff0ac9d) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-040d06b2bd479d108 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-040d06b2bd479d108) | 
| cn\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-07e7d93f94c5189e1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-07e7d93f94c5189e1) | 
| cn\-northwest\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-035dea68ecc98cde0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-035dea68ecc98cde0) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0d36fb88763720f1e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0d36fb88763720f1e) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0df9a0dd16acd8417 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0df9a0dd16acd8417) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0c64d9a31291b49cd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0c64d9a31291b49cd) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0a1a37873049d53a4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0a1a37873049d53a4) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0eed1dfa08262b48e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0eed1dfa08262b48e) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-0ebd69c701091334a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0ebd69c701091334a) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-08528f2e9eef0c497 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-08528f2e9eef0c497) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190603\-x86\_64\-ebs | ami\-b4c0b9d5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-b4c0b9d5) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.28.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.28.1-2`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0ff3b201db8718817 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0ff3b201db8718817) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-081477ad0bee1101b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-081477ad0bee1101b) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0d0c7c92665367ada | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0d0c7c92665367ada) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0f1e071f7210c9fef | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0f1e071f7210c9fef) | 
| ap\-east\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-01d16974f94e9fb7d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-east-1#LaunchInstanceWizard:ami=ami-01d16974f94e9fb7d) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0267b4c44b04cac22 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0267b4c44b04cac22) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-08c9b73959f79470b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-08c9b73959f79470b) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-08e11f14a85a5cc04 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-08e11f14a85a5cc04) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0e824b06f84f8707d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0e824b06f84f8707d) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-09beedd47899d6734 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-09beedd47899d6734) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0ff40b496ddea60b9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0ff40b496ddea60b9) | 
| cn\-north\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-00bff873c7c4d24a7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-north-1#LaunchInstanceWizard:ami=ami-00bff873c7c4d24a7) | 
| cn\-northwest\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0528a17d406522fb9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=cn-northwest-1#LaunchInstanceWizard:ami=ami-0528a17d406522fb9) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0cb576474c127a755 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0cb576474c127a755) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0eef92b5452f95e82 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0eef92b5452f95e82) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-03c692d50c3f91f02 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-03c692d50c3f91f02) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0e1efa8625b557401 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e1efa8625b557401) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-03d45a3e4cd32e19c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-03d45a3e4cd32e19c) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0f7d0d3d1090e86ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0f7d0d3d1090e86ba) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-0f53d0f011a34f4cf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0f53d0f011a34f4cf) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.s\-amazon\-ecs\-optimized | ami\-60cab301 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-60cab301) | 

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

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)
+ [AMI Storage Configuration](ecs-ami-storage-config.md)