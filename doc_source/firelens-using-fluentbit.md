# Using the AWS for Fluent Bit image<a name="firelens-using-fluentbit"></a>

AWS provides a Fluent Bit image with plugins for both CloudWatch Logs and Kinesis Data Firehose\. We recommend using Fluent Bit as your log router because it has a lower resource utilization rate than Fluentd\. For more information, see [CloudWatch Logs for Fluent Bit](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit) and [Amazon Kinesis Firehose for Fluent Bit](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit)\.

The **AWS for Fluent Bit** image is available on [Docker Hub](https://hub.docker.com/r/amazon/aws-for-fluent-bit)\. However, we recommend that you use the following images in Amazon ECR because they provide higher availability\.


| Region Name | Region | Image URI | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  us\-east\-1  |  `906394416424.dkr.ecr.us-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US East \(Ohio\)  |  us\-east\-2  |  `906394416424.dkr.ecr.us-east-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US West \(N\. California\)  |  us\-west\-1  |  `906394416424.dkr.ecr.us-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US West \(Oregon\)  |  us\-west\-2  |  `906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  |  `449074385750.dkr.ecr.ap-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  |  `906394416424.dkr.ecr.ap-south-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  |  `906394416424.dkr.ecr.ap-northeast-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  |  `906394416424.dkr.ecr.ap-southeast-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  |  `906394416424.dkr.ecr.ap-southeast-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  |  `906394416424.dkr.ecr.ap-northeast-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Canada \(Central\)  |  ca\-central\-1  |  `906394416424.dkr.ecr.ca-central-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  |  `906394416424.dkr.ecr.eu-central-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Ireland\)  |  eu\-west\-1  |  `906394416424.dkr.ecr.eu-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(London\)  |  eu\-west\-2  |  `906394416424.dkr.ecr.eu-west-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Paris\)  |  eu\-west\-3  |  `906394416424.dkr.ecr.eu-west-3.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  |  `906394416424.dkr.ecr.eu-north-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  |  `741863432321.dkr.ecr.me-south-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  South America \(São Paulo\)  |  sa\-east\-1  |  `906394416424.dkr.ecr.sa-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  |  `161423150738.dkr.ecr.us-gov-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  |  `161423150738.dkr.ecr.us-gov-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  China \(Beijing\)  |  cn\-north\-1  |  `128054284489.dkr.ecr.cn-north-1.amazonaws.com.cn/aws-for-fluent-bit:latest`  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  |  `128054284489.dkr.ecr.cn-northwest-1.amazonaws.com.cn/aws-for-fluent-bit:latest`  | 