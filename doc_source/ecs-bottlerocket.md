# Using Bottlerocket with Amazon ECS<a name="ecs-bottlerocket"></a>

Bottlerocket is a Linux\-based open source operating system that is purpose\-built by AWS for running containers\. For more information, see [Bottlerocket OS](https://github.com/bottlerocket-os/bottlerocket) on GitHub\.

An Amazon ECS\-optimized AMI variant of the Bottlerocket operating system is provided as an AMI you can use when launching Amazon ECS container instances\. For a detailed walkthrough of how to get started with the Bottlerocket operating system on Amazon ECS, see [Using a Bottlerocket AMI with Amazon ECS](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md) on GitHub\.

You can request new features on the GitHub page\. For more information, see [Bottlerocket Issues](https://github.com/bottlerocket-os/bottlerocket/issues) on GitHub\.

## Amazon ECS\-optimized Bottlerocket AMI variants<a name="ecs-bottlerocket-variants"></a>

You can use the following Amazon ECS\-optimized Bottlerocket AMI variants for your Amazon EC2 instances\.
+ `aws-ecs-1`
+ `aws-ecs-1-nvidia`

  For more information about the `aws-ecs-1-nvidia` variant, see [Announcing NVIDIA GPU support for Bottlerocket on Amazon ECS](https://aws.amazon.com/blogs/containers/announcing-nvidia-gpu-support-for-bottlerocket-on-amazon-ecs/)\.

## Considerations<a name="ecs-bottlerocket-considerations"></a>

Consider the following when using a Bottlerocket AMI with Amazon ECS\.
+ Bottlerocket is optimized for container workloads and has a focus on security\. It does not include a package manager and is immutable by default\. For information about the security features and guidance, see [Security Features](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_FEATURES.md) and [Security Guidance](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_GUIDANCE.md) on the GitHub website\.
+ Amazon EC2 instances with x86 or arm64 processors are supported\.
+ Amazon EC2 instances with Inferentia chips are not supported\.
+ The `awsvpc` network mode is supported when using Bottlerocket AMI version `1.1.0` or later\.
+ The `initProcessEnabled` task definition parameter is not supported\.
+ The following features are not supported:
  + App Mesh in task definitions
  + ECS Anywhere
  + ECS Exec
  + Amazon EFS file system volumes
  + Amazon EFS in encrypted mode and `awsvpc` network mode
  + Elastic Inference
  + FireLens in task definitions

## Retrieving an Amazon ECS\-optimized Bottlerocket AMI<a name="ecs-bottlerocket-retrieve-ami"></a>

You can use one of the following ways to retrieve an Amazon ECS\-optimized Bottlerocket AMI variant\.
+ Use AWS Systems Manager parameters in a AWS CLI command
+ Click the AMI link for a specific region, variant, and architecture in the tables on this page

### Retrieving the `aws-ecs-1` Bottlerocket AMI variant<a name="ecs-bottlerocket-aws-ecs-1-variant"></a>

#### Use AWS Systems Manager parameters in a AWS CLI command<a name="w184aac20c19c25c13b7b1b3"></a>

Use AWS Systems Manager parameters in the following AWS CLI command to retrieve the latest stable `aws-ecs-1` Bottlerocket AMI variant by Region and architecture\. To retrieve a version other than the latest, replace `latest` with the version number\.
+ For the 64\-bit \(`x86_64`\) architecture:

  ```
  aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id" --query Parameter.Value --output text
  ```
+ For the 64\-bit ARM \(`arm64`\) architecture:

  ```
  aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id" --query Parameter.Value --output text
  ```

#### Click the AMI link<a name="w184aac20c19c25c13b7b1b5"></a>

The following table provides a link to retrieve the latest Amazon ECS\-optimized Bottlerocket AMI variant `aws-ecs-1`, by Region and architecture\.


|  Region Name  |  Region  |  x86\_64 AMI ID  |  arm64 AMI ID  | 
| --- | --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-east-1#)  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-east-2#)  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-east-2#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-west-1#)  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-west-2#)  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  |  `af-south-1`  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=af-south-1#)  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=af-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-east-1#)  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-northeast-1#)  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-northeast-2#)  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Osaka\)  |  `ap-northeast-3`  |  [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-northeast-3#)  |  [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-northeast-3#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-south-1#)  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-southeast-1#)  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-southeast-2#)  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  |  `ap-southeast-3`  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-southeast-3#)  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ca-central-1#)  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-central-1#)  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  |  `eu-north-1`  |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-north-1#)  |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-1#)  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  |  `eu-west-2`  |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-2#)  |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-3#)  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-west-3#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-south-1#)  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=eu-south-1#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=me-south-1#)  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=me-south-1#)  | 
|  Middle East \(UAE\)  |  `me-central-1`  |  [View AMI ID](https://me-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=me-central-1#)  |  [View AMI ID](https://me-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=me-central-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=sa-east-1#)  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  |  `us-gov-east-1`  |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-gov-east-1#)  |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  |  `us-gov-west-1`  |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-gov-west-1#)  |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=us-gov-west-1#)  | 

### Retrieving the `aws-ecs-1-nvidia` Bottlerocket AMI variant<a name="ecs-bottlerocket-aws-ecs-1-nvidia-variants"></a>

#### Use AWS Systems Manager parameters a AWS CLI command<a name="w184aac20c19c25c13b7b3b3"></a>

Use AWS Systems Manager parameters in the following AWS CLI command to retrieve the latest stable `aws-ecs-1-nvidia` Bottlerocket AMI variant by Region and architecture\. To retrieve a version other than the latest, replace `latest` with the version number\.
+ For the 64\-bit \(`x86_64`\) architecture:

  ```
  aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id" --query Parameter.Value --output text
  ```
+ For the 64\-bit ARM \(`arm64`\) architecture:

  ```
  aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id" --query Parameter.Value --output text
  ```

#### Click the AMI link<a name="w184aac20c19c25c13b7b3b5"></a>

The following table provides a link to retrieve the latest Amazon ECS\-optimized Bottlerocket AMI variant `aws-ecs-1-nvidia`, by Region and architecture\.


|  Region Name  |  Region  |  x86\_64 AMI ID  |  arm64 AMI ID  | 
| --- | --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-east-1#)  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-east-2#)  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-east-2#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-west-1#)  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-west-2#)  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  |  `af-south-1`  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=af-south-1#)  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=af-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-east-1#)  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-northeast-1#)  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-northeast-2#)  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Osaka\)  |  `ap-northeast-3`  |  [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-northeast-3#)  |  [View AMI ID](https://ap-northeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-northeast-3#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-south-1#)  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-southeast-1#)  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-southeast-2#)  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Jakarta\)  |  `ap-southeast-3`  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ap-southeast-3#)  |  [View AMI ID](https://ap-southeast-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ap-southeast-3#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=ca-central-1#)  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-central-1#)  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  |  `eu-north-1`  |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-north-1#)  |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-west-1#)  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  |  `eu-west-2`  |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-west-2#)  |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-west-3#)  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-west-3#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=eu-south-1#)  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=eu-south-1#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=me-south-1#)  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=me-south-1#)  | 
|  Middle East \(UAE\)  |  `me-central-1`  |  [View AMI ID](https://me-central.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=me-central-1#)  |  [View AMI ID](https://me-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=me-central-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=sa-east-1#)  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  |  `us-gov-east-1`  |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-gov-east-1#)  |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  |  `us-gov-west-1`  |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=us-gov-west-1#)  |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id/description?region=us-gov-west-1#)  | 