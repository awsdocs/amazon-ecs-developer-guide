# API failure reasons<a name="api_failures_messages"></a>

When an API action that you have triggered through the Amazon ECS API, console, or the AWS CLI exits with a `failures` error message, the following may assist in troubleshooting the cause\. The failure will return a reason and the Amazon Resource Name \(ARN\) of the resource associated with the failure\.

Many resources are Region\-specific, so when using the console ensure that you set the correct Region for your resources\. When using the AWS CLI, make sure that your AWS CLI commands are being sent to the correct region with the `--region region` parameter\.

For more information about the structure of the `Failure` data type, see [Failure](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Failure.html) in the *Amazon Elastic Container Service API Reference*\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/api_failures_messages.html)