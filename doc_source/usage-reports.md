# Amazon ECS usage reports<a name="usage-reports"></a>

AWS provides a free reporting tool called Cost Explorer that enables you to analyze the cost and usage of your Amazon ECS resources\.

Cost Explorer is a free tool that you can use to view charts of your usage and costs\. You can view data from the last 13 months, and forecast how much you are likely to spend for the next three months\. You can use Cost Explorer to see patterns in how much you spend on AWS resources over time\. For example, you can use it to identify areas that need further inquiry and see trends that you can use to understand your costs\. You also can specify time ranges for the data, and view time data by day or by month\.

The metering data in your Cost and Usage Report shows usage across all of your Amazon ECS tasks\. The metering data includes CPU usage as `vCPU-Hours` and memory usage as `GB-Hours` for each task that was run\. How that data is presented depends on the launch type of the task\.

For tasks using the Fargate launch type, the `lineItem/Operation` column shows `FargateTask` and you will see the cost associated with each task\.

For tasks using the EC2 launch type, the `lineItem/Operation` column shows `ECSTask-EC2` and the tasks don't have a direct cost associated with them\. The metering data shown in the report, such as memory usage, represents the total resources reserved by the task over the billing period indicated\. These values can be used to determine the cost of your underlying cluster of Amazon EC2 instances\. The cost and usage data for your Amazon EC2 instances are listed separately under the Amazon EC2 service\. 

You can also use the Amazon ECS\-managed tags to identify the service or cluster that each task belongs to\. For more information, see [Tagging your resources for billing](ecs-using-tags.md#tag-resources-for-billing)\.

**Important**  
The metering data is only viewable for tasks launched on or after November 16, 2018\. Tasks launched before this date don't show metering data\.

Here's an example of some of the fields you can sort cost allocation data by using Cost Explorer\.
+ Cluster name
+ Service name
+ Resource tags
+ Launch type
+ Region
+ Usage type

For more information about creating an AWS Cost and Usage Report, see [AWS Cost and Usage Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) in the *AWS Billing and Cost Management User Guide*\.