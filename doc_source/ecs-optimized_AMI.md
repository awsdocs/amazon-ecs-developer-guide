# Amazon ECS\-optimized AMIs<a name="ecs-optimized_AMI"></a>

The basic Amazon Elastic Container Service \(Amazon ECS\) container instance specification consists of the following:

**Required**
+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.
+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.
+ A Docker daemon running at least version 1\.9\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS agent version that you are using\. For more information, see [Amazon ECS Container Agent Versions](ecs-agent-versions.md)\.

**Recommended**
+ An initialization and nanny process to run and monitor the Amazon ECS agent\. The Amazon ECS\-optimized AMIs use the `ecs-init` RPM to manage the agent\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

The Amazon ECS\-optimized AMIs are preconfigured with these requirements and recommendations\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your container instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Amazon ECS vends AMIs that are optimized for the service in the following variants\.
+ **Amazon ECS\-optimized Amazon Linux 2 AMI** – Recommended for launching your Amazon ECS container instances in most cases\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI** – Recommended for launching your Amazon ECS container instances when using the Amazon EC2 A1 instance type, which is powered by Arm\-based AWS Graviton Processors\. For more information, see [General Purpose Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ **Amazon ECS GPU\-optimized AMI** – Recommended for launching your Amazon ECS container instances when working with GPU workloads\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.
+ **Amazon ECS\-optimized Amazon Linux AMI** – This AMI is based off of Amazon Linux 1\. We recommend that you migrate your workloads to the Amazon ECS\-optimized Amazon Linux 2 AMI\. Support for the Amazon ECS\-optimized Amazon Linux AMI ends no later than December 31, 2020\.
+ **Amazon ECS\-optimized Windows 2019 Full AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows 2019 Core AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows 1909 Core AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows 2016 Full AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.

Although you can create your own container instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI name, Amazon ECS container agent version, and ECS runtime version which includes the Docker version, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

View the AMI IDs on one of the following tabs, according to the variant you choose\.

------
#### [ Amazon Linux 2 ]

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` | [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-northeast-1#) | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` | [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-northeast-2#) | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` | [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-south-1#) | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` | [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-southeast-1#) | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` | [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-southeast-2#) | 
|  Canada \(Central\)  | `ca-central-1` | [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ca-central-1#) | 
|  Europe \(Frankfurt\)  | `eu-central-1` | [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-central-1#) | 
|  Europe \(Stockholm\)  | `eu-north-1` | [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-north-1#) | 
|  Europe \(Ireland\)  | `eu-west-1` | [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-1#) | 
|  Europe \(London\)  | `eu-west-2` | [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-2#) | 
|  Europe \(Paris\)  | `eu-west-3` | [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-3#) | 
|  Middle East \(Bahrain\)  | `me-south-1` | [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=me-south-1#) | 
|  South America \(São Paulo\)  | `sa-east-1` | [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=sa-east-1#) | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-gov-east-1#) | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-gov-west-1##) | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=cn-northwest-1#)  | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-west-1#)  | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  | us\-west\-1 |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  | ap\-east\-1 |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  | cn\-north\-1 |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  | cn\-northwest\-1 |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-northwest-1#)  | 

------
#### [ Amazon Linux AMI ]

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-northwest-1#)  | 

------
#### [ Windows Server 2019 Full ]

The following table lists the current Amazon ECS\-optimized Windows 2019 Full AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=cn-northwest-1#)  | 

------
#### [ Windows Server 2019 Core ]

The following table lists the current Amazon ECS\-optimized Windows 2019 Core AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

------
#### [ Windows Server 1909 Core ]

The following table lists the current Amazon ECS\-optimized Windows 1909 Core AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

------
#### [ Windows Server 2016 Full ]

The following table lists the current Amazon ECS\-optimized Windows 2016 Full AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=cn-northwest-1#)  | 

------

**Topics**
+ [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)
+ [AMI Storage Configuration](ecs-ami-storage-config.md)