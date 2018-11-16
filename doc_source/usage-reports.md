# Amazon ECS Usage Reports<a name="usage-reports"></a>

AWS provides a free reporting tool called Cost Explorer that enables you to analyze the cost and usage of your Amazon ECS resources\.

Cost Explorer is a free tool that you can use to view charts of your usage and costs\. You can view data from the last 13 months, and forecast how much you are likely to spend for the next three months\. You can use Cost Explorer to see patterns in how much you spend on AWS resources over time, identify areas that need further inquiry, and see trends that you can use to understand your costs\. You also can specify time ranges for the data, and view time data by day or by month\.

The metering data in your Cost & Usage Report shows usage across all of your Amazon ECS tasks\. The metering data includes `vCPU-Hours` and memory `GB-Hours` for each task that was run\. For tasks using the Fargate launch type, you will also see the cost associated with those tasks\. For tasks using the EC2 launch type, the tasks will not have a cost associated with them, but you can use the the vCPU and/or memory usage to allocate the cost of your underlying cluster of Amazon EC2 instances\. You can also use the Amazon ECS managed tags to identify the service or cluster that each task belongs to\. For more information, see [Tagging Your Resources for Billing](ecs-using-tags.md#tag-resources-for-billing)\.

**Important**  
The metering data is only viewable for tasks launched on or after November 16, 2018\. Tasks launched prior to this date will not show metering data\.

Here's an example of some of the fields you can sort cost allocation data by when using Cost Explorer:
+ Cluster name
+ Service name
+ Resource tags
+ Launch type
+ Region
+ Usage type

For more information about creating an AWS Cost and Usage Report, see [AWS Cost and Usage Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) in the *AWS Billing and Cost Management User Guide*\.