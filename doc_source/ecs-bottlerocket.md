# Using Bottlerocket with Amazon ECS<a name="ecs-bottlerocket"></a>


|  | 
| --- |
| The Bottlerocket AMI variant is in developer preview release for Amazon ECS and is subject to change\. | 

Bottlerocket is a Linux\-based open source operating system that is purpose\-built by AWS for running containers\. For more information, see [Bottlerocket on GitHub](https://github.com/bottlerocket-os/bottlerocket)\.

An Amazon ECS\-optimized AMI variant of the Bottlerocket operating system is provided as an AMI you can use when launching Amazon ECS container instances\. For a detailed walkthrough of how to get started with the Bottlerocket operating system on Amazon ECS, see [Using a Bottlerocket AMI with Amazon ECS ](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md)\.

You can request new features on the GitHub page\. For more information, see [Bottlerocket on GitHub](https://github.com/bottlerocket-os/bottlerocket/issues)\.

## Considerations<a name="ecs-bottlerocket-considerations"></a>

The following should be considered when using the Bottlerocket AMI with Amazon ECS\.
+ The Amazon ECS variant of the Bottlerocket AMI is not supported in the following Regions:
  + China \(Beijing\) \(`cn-north-1`\)
  + China \(Ningxia\) \(`cn-northwest-1`\)
  + AWS GovCloud \(US\-East\) \(`us-gov-east-1`\)
  + AWS GovCloud \(US\-West\) \(`us-gov-west-1`\)
+ Amazon EC2 instances with x86 or arm64 processors are supported\. Amazon EC2 instances with GPUs or Inferentia chips are not supported\.
+ The `awsvpc` network mode is not supported\.
+ Using Amazon EFS file system volumes are not supported\.
+ The `initProcessEnabled` task definition parameter is not supported\.

## Retrieving the Bottlerocket AMI<a name="ecs-bottlerocket-retrieve"></a>

The Amazon ECS variant of the Bottlerocket AMI can be retrieved using a Systems Manager parameter\. The following is the format of the parameter name\.

```
/aws/service/bottlerocket/aws-ecs-1/x86_64/latest
```

You can retrieve the latest stable Bottlerocket AMI using the AWS CLI with the following command\.

```
aws ssm get-parameters --name "/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id" --region us-east-1
```

The following table provides a link to retrieve the latest AMI ID of the Amazon ECS variant of the Bottlerocket operating system by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-east-2#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=us-west-2#)  | 
|  Africa \(Cape Town\)  |  `af-south-1`  |  [View AMI ID](https://af-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=af-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ap-southeast-2#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  |  `eu-north-1`  |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-north-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-1#)  | 
|  Europe \(London\)  |  `eu-west-2`  |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-2#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-west-3#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=eu-south-1#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=sa-east-1#)  | 