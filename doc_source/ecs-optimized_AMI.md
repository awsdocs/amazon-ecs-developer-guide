# Amazon ECS\-optimized AMIs<a name="ecs-optimized_AMI"></a>

An Amazon ECS container instance specification consists of the following components\.

**Required**
+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.
+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.
+ A Docker daemon running at least version 1\.9\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS container agent version that you are using\.

**Recommended**
+ An initialization and nanny process to run and monitor the Amazon ECS container agent\. The Amazon ECS\-optimized AMIs use the `ecs-init` RPM to manage the agent\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

The Amazon ECS\-optimized AMIs are preconfigured with these requirements and recommendations\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your container instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Although you can create your own container instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI name, Amazon ECS container agent version, and ECS runtime version which includes the Docker version, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

## Amazon ECS\-optimized AMI variants<a name="ecs-optimized-ami-variants"></a>

Amazon ECS vends AMIs that are optimized for the service in the following variants\.

### Linux variants<a name="ecs-optimized-ami-linux-variants"></a>
+ **Amazon ECS\-optimized Amazon Linux 2 AMI** – Recommended for launching your Amazon ECS container instances in most cases\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 A1 instance type instances, which are powered by Arm\-based AWS Graviton Processors\. For more information, see [General Purpose Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ **Amazon ECS GPU\-optimized AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 GPU\-based instances\. It comes pre\-configured with NVIDIA kernel drivers and a Docker GPU runtime which makes running workloads that take advantage of GPUs on Amazon ECS\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 Inf1 instances\. It comes pre\-configured with AWS Inferentia drivers and the AWS Neuron runtime for Docker which makes running machine learning inference workloads easier on Amazon ECS\. For more information, see [Working with inference workloads on Amazon ECS](ecs-inference.md)\.
+ **Amazon ECS\-optimized Amazon Linux AMI** – This AMI is based off of Amazon Linux\. We recommend that you migrate your workloads to the Amazon ECS\-optimized Amazon Linux 2 AMI\. Support for the Amazon ECS\-optimized Amazon Linux AMI is the same as the Amazon Linux AMI\. For more information, see [Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/)\.

### Windows variants<a name="ecs-optimized-ami-windows-variants"></a>
+ **Amazon ECS\-optimized Windows Server 2019 Full AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows Server 2019 Core AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows Server 1909 Core AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows Server 2016 Full AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows Server 2004 Core AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows containers](ECS_Windows.md)\.

Windows Server 2019 and Windows Server 2016 are Long\-Term Servicing Channel \(LTSC\) releases\. Windows Server, version 1909, is a Semi\-Annual Channel \(SAC\) release\. For more information, see [Windows Server release information](https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info)\.

## Linux Amazon ECS\-optimized AMIs<a name="ecs-optimized-ami-linux"></a>

The following are the details for retrieving the AMI IDs for each of the Linux variants of the Amazon ECS\-optimized AMI\.

### Amazon Linux 2<a name="al2ami"></a>

The latest Amazon ECS\-optimized Amazon Linux 2 AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  |  `af-south-1`  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=af-south-1#)  | 
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
|  Europe \(Milan\)  | `eu-south-1` | [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-south-1#) | 
|  Europe \(Paris\)  | `eu-west-3` | [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-3#) | 
|  Middle East \(Bahrain\)  | `me-south-1` | [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=me-south-1#) | 
|  South America \(São Paulo\)  | `sa-east-1` | [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=sa-east-1#) | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-gov-east-1#) | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=us-gov-west-1##) | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package, see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux 2 \(arm64\)<a name="al2arm64ami"></a>

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-west-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package, see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux 2 \(GPU\)<a name="gpuami"></a>

You can retrieve the current Amazon ECS GPU\-optimized AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended
```

The following table provides a link to retrieve the current Amazon ECS GPU\-optimized AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  | us\-west\-1 |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  | af\-south\-1 |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=af-south-1#)  | 
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
|  Europe \(Milan\)  | eu\-south\-1 |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  | cn\-north\-1 |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  | cn\-northwest\-1 |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, the `ecs-init` package, and NVIDIA driver see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux 2 \(Inferentia\)<a name="infami"></a>

You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  | us\-east\-2 |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-2#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux AMI<a name="alami"></a>

You can retrieve the current Amazon ECS\-optimized Amazon Linux AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  |  `af-south-1`  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=af-south-1#)  | 
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
|  Europe \(Milan\)  | eu\-south\-1 |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

## Windows Amazon ECS\-optimized AMIs<a name="ecs-optimized-ami-windows"></a>

The following are the details for retrieving the AMI IDs for each of the Windows variants of the Amazon ECS\-optimized AMI\. You can subscribe to the Windows AMI Amazon SNS topics to be notified when a new AMI is released or an AMI version is marked private\. For more information, see [Windows Amazon ECS\-optimized AMIs](ECS-AMI-SubscribeTopic.md#sns-topic-windows)\.

**Important**  
To ensure that customers have the latest security updates by default, Amazon ECS maintains at least the last three Windows Amazon ECS\-optimized AMIs\. After releasing new Windows Amazon ECS\-optimized AMIs, Amazon ECS makes the Windows Amazon ECS\-optimized AMIs that are older private\. If there is a private AMI that you need access to, let us know by filing a ticket with Cloud Support\.

### Windows Server 2019 Full<a name="windows-2019-full-ami"></a>

The current Amazon ECS\-optimized Windows Server 2019 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 2019 Full AMI IDs by Region\.


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

For a full list of current and previous versions of the Windows Server 2019 Full and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 2019 Full<a name="windows-2019-core-ami"></a>

The current Amazon ECS\-optimized Windows Server 2019 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 2019 Core AMI IDs by Region\.


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

For a full list of current and previous versions of the Windows Server 2019 Core and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 1909 Core<a name="windows-1909-core-ami"></a>

The current Amazon ECS\-optimized Windows Server 1909 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 1909 Core AMI IDs by Region\.


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

For a full list of current and previous versions of the Amazon ECS\-optimized Windows Server 1909 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 2016 Full<a name="windows-2016-full-ami"></a>

The current Amazon ECS\-optimized Windows Server 2016 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 2016 Full AMI IDs by Region\.


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

For a full list of current and previous versions of the Amazon ECS\-optimized Windows Server 2016 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 2004 Core<a name="windows-2004-ami"></a>

The current Amazon ECS\-optimized Windows Server 2004 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 2004 Core AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2 |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1 |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1 |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | ca\-central\-1 |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | eu\-north\-1 |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | eu\-west\-2 |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | eu\-west\-3 |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | me\-south\-1 |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | sa\-east\-1 |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | us\-gov\-east\-1 |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | us\-gov\-west\-1 |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Windows Server 2004 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-windows)\.