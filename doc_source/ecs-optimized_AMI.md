# Amazon ECS\-optimized AMI<a name="ecs-optimized_AMI"></a>

Amazon ECS provides the Amazon ECS\-optimized AMIs that are preconfigured with the requirements and recommendations to run your container workloads on Amazon ECS Linux instances\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your Amazon EC2 instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Although you can create your own Amazon EC2 instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI name, Amazon ECS container agent version, and Amazon ECS runtime version which includes the Docker version, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

The following variants of the Amazon ECS\-optimized AMI are available for your Amazon EC2 instances\.
+ **Amazon ECS\-optimized Amazon Linux 2 AMI** – For most cases, recommended for launching your Amazon EC2 instances for your Amazon ECS workloads\.

  The Amazon ECS\-optimized Amazon Linux 2 AMI does not come with the AWS CLI preinstalled\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 instances, which are powered by Arm\-based AWS Graviton/Graviton 2 Processors, for your Amazon ECS workloads\. For more information, see [General Purpose Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

  The Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI does not come with the AWS CLI preinstalled\.
+ **Amazon ECS GPU\-optimized AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 GPU\-based instances for your Amazon ECS workloads\. It comes pre\-configured with NVIDIA kernel drivers and a Docker GPU runtime which makes running workloads that take advantage of GPUs on Amazon ECS\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.

  The Amazon ECS GPU\-optimized AMI does not come with the AWS CLI preinstalled\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI** – Based on Amazon Linux 2, this AMI is recommended for use when launching your Amazon EC2 Inf1 instances\. It comes pre\-configured with AWS Inferentia drivers and the AWS Neuron runtime for Docker which makes running machine learning inference workloads easier on Amazon ECS\. For more information, see [Using inference Inf1 instances on Amazon ECS](ecs-inference.md)\.

  The Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI does not come with the AWS CLI preinstalled\.
+ **Amazon ECS\-optimized Amazon Linux 2022 AMI** – Amazon Linux 2022 is the next generation of Amazon Linux from AWS\. For most cases, recommended for launching your Amazon EC2 instances for your Amazon ECS workloads\. For more information, see [What is Amazon Linux 2022](https://docs.aws.amazon.com/linux/al2022/ug/what-is-amazon-linux.html) in the *Amazon Linux 2022 User Guide*\.
**Important**  
The Amazon ECS\-Optimized Amazon Linux 2022 AMI is in preview and subject to change\.
+ **Amazon ECS\-optimized Amazon Linux AMI** – This AMI is based off of Amazon Linux\. We recommend that you migrate your workloads to the Amazon ECS\-optimized Amazon Linux 2 AMI\. Support for the Amazon ECS\-optimized Amazon Linux AMI is the same as the Amazon Linux AMI\. For more information, see Amazon Linux AMI\.
**Important**  
On April 15, 2021, the Amazon ECS\-optimized Amazon Linux AMI ended its standard support phase and entered a maintenance support phase\. In the maintenance support phase, Amazon ECS will continue providing critical and important security updates for a reduced list of packages\. During this period, Amazon ECS will no longer add support for new EC2 instance types, new services and features, and new packages\. Instead, Amazon ECS will provide updates only for critical and important security fixes that apply to a reduced set of packages\. Maintenance support period will end on June 30, 2023\.

## Amazon ECS\-optimized AMI changelog<a name="ecs-optimized-ami-linux-releasenotes"></a>

Amazon ECS provides a changelog for the Linux variant of the Amazon ECS\-optimized AMI on GitHub\. For more information, see [Changelog](https://github.com/aws/amazon-ecs-ami/blob/main/CHANGELOG.md)\.

The Linux variants of the Amazon ECS\-optimized AMI use the Amazon Linux 2 AMI as their base\. The Amazon Linux 2 source AMI name for each variant can be retrieved by querying the Systems Manager Parameter Store API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\. The Amazon Linux 2 AMI release notes are available as well\. For more information, see [Amazon Linux 2 release notes](http://aws.amazon.com/amazon-linux-2/release-notes/)\.

## Linux Amazon ECS\-optimized AMI<a name="ecs-optimized-ami-linux"></a>

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
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  | [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-northeast-1#) | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  | [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-northeast-2#) | 
|  Asia Pacific \(Osaka\)  |  `ap-northeast-3`  | [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-northeast-3#) | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  | [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-south-1#) | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  | [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-southeast-1#) | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  | [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-southeast-2#) | 
|  Asia Pacific \(Jakarta\)  |  `ap-southeast-3`  | [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ap-southeast-3#) | 
|  Canada \(Central\)  |  `ca-central-1`  | [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=ca-central-1#) | 
|  Europe \(Frankfurt\)  | `eu-central-1` | [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-central-1#) | 
|  Europe \(Stockholm\)  | `eu-north-1` | [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-north-1#) | 
|  Europe \(Ireland\)  | `eu-west-1` | [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-1#) | 
|  Europe \(London\)  | `eu-west-2` | [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-2#) | 
|  Europe \(Milan\)  | `eu-south-1` | [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-south-1#) | 
|  Europe \(Paris\)  | `eu-west-3` | [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=eu-west-3#) | 
|  Middle East \(Bahrain\)  | `me-south-1` | [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=me-south-1#) | 
|  Middle East \(UAE\)  | `me-central-1` | [View AMI ID](https://me-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id/description?region=me-central-1#) | 
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
|  US East \(N\. Virginia\)  | `us-east-1` |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  | `us-west-1` |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  | `us-west-2` |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  | `ap-southeast-3` |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-west-3#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-north-1#)  | 
|  Europe \(Milan\)  | `eu-south-1` |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=eu-south-1#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=me-south-1#)  | 
|  Middle East \(UAE\)  | `me-central-1` |  [View AMI ID](https://me-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=me-central-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-gov-east-1#) | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` | [View AMI ID](https://console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=us-gov-west-1##) | 
|  China \(Beijing\)  | `cn-north-1` |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=cn-north-1##)  | 
|  China \(Ningxia\)  | `cn-northwest-1` |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended/image_id/description?region=cn-northwest-1#)  | 

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
|  US East \(N\. Virginia\)  | `us-east-1` |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-east-1#)  | 
|  US West \(N\. California\)  | `us-west-1` |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  | `us-west-2` |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  | `af-south-1` |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=af-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  | `ap-east-1` |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  | `ap-southeast-3` |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-2#)  | 
|  Europe \(Milan\)  | `eu-south-1` |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  | `cn-north-1` |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  | `cn-northwest-1` |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, the `ecs-init` package, and NVIDIA driver see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux 2 \(Inferentia\)<a name="infami"></a>

You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI IDs by Region\.


|  Region name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-2#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  |  `ap-southeast-3`  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux 2022<a name="al2022ami"></a>

**Important**  
The Amazon ECS\-Optimized Amazon Linux 2022 AMI is in preview and subject to change\.

You can retrieve the current Amazon ECS\-optimized Amazon Linux 2022 AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2022/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2022 AMI IDs by Region\.


|  Region name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=us-east-2#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  |  `ap-southeast-3`  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2022/recommended/image_id/description?region=sa-east-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux 2022 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.

### Amazon Linux AMI<a name="alami"></a>

**Important**  
On April 15, 2021, the Amazon ECS\-optimized Amazon Linux AMI ended its standard support phase and entered a maintenance support phase\. In the maintenance support phase, Amazon ECS will continue providing critical and important security updates for a reduced list of packages\. During this period, Amazon ECS will no longer add support for new EC2 instance types, new services and features, and new packages\. Instead, Amazon ECS will provide updates only for critical and important security fixes that apply to a reduced set of packages\. Maintenance support period will end on June 30, 2023\.

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
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Osaka\)  | `ap-northeast-3` |  [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-northeast-3#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  | `ap-southeast-3` |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-2#)  | 
|  Europe \(Milan\)  | `eu-south-1` |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id/description?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Amazon Linux AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\.