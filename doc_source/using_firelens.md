# Custom Log Routing<a name="using_firelens"></a>


****  

|  | 
| --- |
| FireLens for Amazon ECS is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback by commenting on [https://github\.com/aws/containers\-roadmap/issues/10](https://github.com/aws/containers-roadmap/issues/10) | 

FireLens for Amazon ECS enables you to use task definition parameters to route logs to an AWS service or partner destination for log storage and analytics\. FireLens works with [Fluentd](https://www.fluentd.org/) and [Fluent Bit](https://fluentbit.io/)\. We provide the AWS for Fluent Bit plugin or you can bring your own Fluentd or Fluent Bit output plugin to use\.

During the public preview, we are providing FireLens with a basic set of functionality to allow you to test it out, and give us feedback\. Once we announce the general availability of FireLens, it will be ready for production workloads and will enable further use cases\.

Creating Amazon ECS task definitions with a FireLens configuration is supported via the AWS SDKs, AWS CLI, and AWS Management Console\. When using the AWS Management Console to register a new task definition, you must use the **Configure via JSON** option\.

**Note**  
FireLens is available for Amazon ECS tasks using both the EC2 and Fargate launch types\.

For more information, see [https://github\.com/aws/containers\-roadmap/tree/master/preview\-programs/firelens](https://github.com/aws/containers-roadmap/tree/master/preview-programs/firelens)\.