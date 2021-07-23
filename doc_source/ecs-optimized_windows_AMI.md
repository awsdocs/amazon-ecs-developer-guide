# Amazon ECS\-optimized AMI<a name="ecs-optimized_windows_AMI"></a>

The Amazon ECS\-optimized AMIs are preconfigured with the necessary compnents that you need to run ECS workloads\. Although you can create your own container instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI name, Amazon ECS container agent version, and ECS runtime version which includes the Docker version, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_windows_AMI.md)\.

## Amazon ECS\-optimized AMI variants<a name="ecs-optimized-ami-variants"></a>

The following Windows Server variants of the Amazon ECS\-optimized AMI are available for your Amazon EC2 instances\. For more information, see [Windows containers](ECS_Windows.md)\.
+ **Amazon ECS\-optimized Windows Server 2019 Full AMI** 
+ **Amazon ECS\-optimized Windows Server 2019 Core AMI** 
+ **Amazon ECS\-optimized Windows Server 2004 Core AMI** 
+ **Amazon ECS\-optimized Windows Server 20H2 Core AMI**
+ **Amazon ECS\-optimized Windows Server 2016 Full AMI**

Windows Server 2019 and Windows Server 2016 are Long\-Term Servicing Channel \(LTSC\) releases\. Windows Server 2004 and Windows Server 20H2 are Semi\-Annual Channel \(SAC\) releases\. For more information, see [Windows Server release information](https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info)\.

## <a name="ecs-optimized-ami-windows"></a>

The following are the details for retrieving the AMI IDs for each of the Windows variants of the Amazon ECS\-optimized AMI\. You can subscribe to the Windows AMI Amazon SNS topics to be notified when a new AMI is released or an AMI version is marked private\. For more information, see [Subscribing to Amazon ECS\-optimized AMI update notifications](ECS-AMI-windows-SubscribeTopic.md)\.

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
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Windows Server 2019 Full and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-windows-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 2019 Core<a name="windows-2019-core-ami"></a>

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
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Windows Server 2019 Core and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-windows-ami-versions.md#ecs-ami-versions-windows)\.

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
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Windows Server 2004 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-windows-ami-versions.md#ecs-ami-versions-windows)\.

### Windows Server 20H2 Core<a name="windows-20H2-core-ami"></a>

The current Amazon ECS\-optimized Windows Server 20H2 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Windows Server 20H2 Core AMI IDs by Region\.


|  Region name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-east-2#)  | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-east-1#)  | 
|  US West \(N\. California\)  |  `us-west-1`  |  [View AMI ID](https://us-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-west-1#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-west-2#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Windows Server 20H2 Core and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-windows-ami-versions.md#ecs-ami-versions-windows)\.

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
|  Asia Pacific \(Tokyo\)  | `ap-northeast-1` |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  | `ap-northeast-2` |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-northeast-2#)  | 
|  Asia Pacific \(Mumbai\)  | `ap-south-1` |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-south-1#)  | 
|  Asia Pacific \(Singapore\)  | `ap-southeast-1` |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  | `ap-southeast-2` |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ap-southeast-2#)  | 
|  Canada \(Central\)  | `ca-central-1` |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  | `eu-central-1` |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-central-1#)  | 
|  Europe \(Stockholm\)  | `eu-north-1` |  [View AMI ID](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-north-1#)  | 
|  Europe \(Ireland\)  | `eu-west-1` |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-1#)  | 
|  Europe \(London\)  | `eu-west-2` |  [View AMI ID](https://eu-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-2#)  | 
|  Europe \(Paris\)  | `eu-west-3` |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  | `me-south-1` |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=me-south-1#)  | 
|  South America \(São Paulo\)  | `sa-east-1` |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized?region=cn-northwest-1#)  | 

For a full list of current and previous versions of the Amazon ECS\-optimized Windows Server 2016 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker see [Windows Amazon ECS\-optimized AMIs versions](ecs-windows-ami-versions.md#ecs-ami-versions-windows)\.