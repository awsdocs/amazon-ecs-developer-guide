# Amazon ECS usage reports<a name="usage-reports"></a>

AWS provides a reporting tool called Cost Explorer that you can use to analyze the cost and usage of your Amazon ECS resources\.

You can use Cost Explorer to view charts of your usage and costs\. You can view data from the last 13 months, and forecast how much you're likely to spend for the next three months\. You can use Cost Explorer to see patterns in how much you spend on AWS resources over time\. For example, you can use it to identify areas that need further inquiry and see trends that you can use to understand your costs\. You also can specify time ranges for the data, and view time data by day or by month\.

The metering data in your Cost and Usage Report shows usage across all of your Amazon ECS tasks\. The metering data includes CPU usage as `vCPU-Hours` and memory usage as `GB-Hours` for each task that was run\. How that data is presented depends on the launch type of the task\.

For tasks using the Fargate launch type, the `lineItem/Operation` column shows `FargateTask` and you will see the cost associated with each task\.

For tasks that use the EC2 launch type, the `lineItem/Operation` column shows `ECSTask-EC2` and the tasks don't have a direct cost associated with them\. The metering data that's shown in the report, such as memory usage, represents the total resources that the task reserved over the billing period that you specify\. You can use this data to determine the cost of your underlying cluster of Amazon EC2 instances\. The cost and usage data for your Amazon EC2 instances are listed separately under the Amazon EC2 service\.

You can also use the Amazon ECS managed tags to identify the service or cluster that each task belongs to\. For more information, see [Tagging your resources for billing](ecs-using-tags.md#tag-resources-for-billing)\.

**Important**  
The metering data is only viewable for tasks that are launched on or after November 16, 2018\. Tasks that are launched before this date don't show metering data\.

The following is an example of some of the fields that you can use to sort cost allocation data in Cost Explorer\.
+ Cluster name
+ Service name
+ Resource tags
+ Launch type
+ AWS Region
+ Usage type

For more information about creating an AWS Cost and Usage Report, see [AWS Cost and Usage Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) in the *AWS Billing User Guide*\.

## Task\-level Cost and Usage Reports<a name="task-cur"></a>

AWS Cost Management can provide CPU and memory usage data in the AWS Cost and Usage Report for the each task on Amazon ECS, including tasks on Fargate and tasks on EC2\. This data is called *Split Cost Allocation Data*\. You can use this data to analyze costs and usage for applications\. Additionally, you can split and allocate the costs to individual business units and teams with cost allocation tags and cost categories\. For more information about *Split Cost Allocation Data*, see [Understanding split cost allocation data](https://docs.aws.amazon.com/cur/latest/userguide/split-cost-allocation-data.html) in the *AWS Cost and Usage Report User Guide*\.

You can opt in to task\-level *Split Cost Allocation Data* for the account in the AWS Cost Management Console\. If you have a management \(payer\) account, you can opt in from the payer account to apply this configuration to every linked account\.

After you set up *Split Cost Allocation Data*, there will be additional columns under the **splitLineItem** header in the report\. For more information see [Split line item details](https://docs.aws.amazon.com/cur/latest/userguide/split-line-item-columns.html) in the *AWS Cost and Usage Report User Guide*\.

For tasks on EC2, this data splits the cost of the EC2 instance based on the resource usage or reservations and the remaining resources on the instance\.

### Prerequisites for Task\-level CURs<a name="task-cur-prereqs"></a>
+ To use *Split Cost Allocation Data*, you must create a report, and select **Split cost allocation data**\. For more information, see [Creating Cost and Usage Reports](https://docs.aws.amazon.com/cur/latest/userguide/cur-create.html) in the *AWS Cost and Usage Report User Guide*\.
+ The minimum Docker version for reliable metrics is Docker version v20\.10\.13 and newer, which is included in Amazon ECS\-optimized AMI 20220607 and newer\.
+ Ensure that the ECS agent has the `ECS_DISABLE_METRICS` configuration set to `false`\. When this setting is `false`, the ECS agent sends metrics to Amazon CloudWatch\. On Linux, this setting is `false` by default and metrics are sent to CloudWatch\. On Windows, this setting is `true` by default, so you must change the setting to `false` to send the metrics to CloudWatch for AWS Cost Management to use\. For more information about ECS agent configuration, see [Amazon ECS container agent configuration](ecs-agent-config.md)\. 

**Note**  
AWS Cost Management calculates the *Split Cost Allocation Data* with the task CPU and memory usage\. AWS Cost Management can use the task CPU and memory reservation instead of the usage, if the usage is unavailable\. If you see the CUR is using the reservations, check that your container instances meet the prerequisites and the task resource usage metrics appear in CloudWatch\.

### Setting up Task\-level Cost and Usage Reports<a name="task-cur-setup"></a>

You can turn on *Split Cost Allocation Data* for ECS in the Cost Management Console, AWS Command Line Interface, or the AWS SDKs\.

There are two steps to use *Split Cost Allocation Data*\. First, you opt in to *Split Cost Allocation Data*\. Second, you include the data in a new or existing report\. For the steps in the Cost Management Console, see [Enabling split cost allocation data](https://docs.aws.amazon.com/cur/latest/userguide/enabling-split-cost-allocation-data.html) in the *AWS Cost and Usage Report User Guide*\.

Then, you can view the report\. You can use the Billing and Cost Management console or view the report files in Amazon Simple Storage Service\.