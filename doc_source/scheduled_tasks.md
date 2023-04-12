# Scheduled tasks<a name="scheduled_tasks"></a>

Amazon ECS supports creating scheduled tasks\. Scheduled tasks use Amazon EventBridge Scheduler\.

**Topics**
+ [Create a scheduled task](#scheduled-task-create)
+ [View your scheduled tasks in the console](#scheduled-task-view)
+ [Edit a scheduled task](#scheduled-task-edit)

## Create a scheduled task<a name="scheduled-task-create"></a>

Scheduled tasks are started by Amazon EventBridge Scheduler schedule, which you can create using the EventBridge console\. Although you can create a scheduled task in the Amazon ECS console, currently the EventBridge console provides more functionality so the following steps walk you through creating an EventBridge Scheduler schedule that starts a scheduled task\.

The EventBridge Scheduler service needs several permissions to run Amazon ECS tasks on your behalf\. For more information about the required service principal and IAM permissions for this role, see [Amazon ECS CloudWatch Events IAM Role](CWE_IAM_role.md)\.

Complete the following steps before you schedule a task:

1. Use the VPC console to get the subnet IDs where the tasks run and the security group IDs for the subnets\. For more information, see [View your subnets](https://docs.aws.amazon.com/vpc/latest/userguide/modify-subnets.html#view-subnet), and [View your security groups](https://docs.aws.amazon.com/vpc/latest/userguide/security-groups.html#viewing-security-groups) in the *Amazon VPC User Guide*\.

1. Configure the EventBridge Scheduler execution role\. For more information, see [Set up the execution role](https://docs.aws.amazon.com/scheduler/latest/UserGuide/setting-up.html#setting-up-execution-role) in the *Amazon EventBridge Scheduler User Guide*\. 

1. Configure the EventBridge Scheduler target\. For more information, see [Set up a target](https://docs.aws.amazon.com/scheduler/latest/UserGuide/setting-up.html#setting-up-target) in the *Amazon EventBridge Scheduler User Guide*\. 

**To create a new schedule using the console**

1. Open the Amazon EventBridge Scheduler console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/scheduler/)\.

1. In the navigation pane, choose **Scheduler**, **Schedule**\.

1.  On the **Schedules** page, choose **Create schedule**\. 

1.  On the **Specify schedule detail** page, in the **Schedule name and description** section, do the following: 

   1. For **Schedule name**, enter a name for your schedule\. For example, **MyTestSchedule** 

   1. \(Optional\) For **Description**, enter a description for your schedule\. For example, **My first schedule**\.

   1. For **Schedule group**, choose a schedule group from the drop down options\. If you do not have a group, choose **default**\. To create a new schedule group, choose **create your own schedule**\. 

      You use schedule groups to add tags to groups of schedules\. 

1. 

   1. Choose your schedule options\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduled_tasks.html)

1. \(Optional\) If you chose **Recurring schedule** in the previous step, in the **Timeframe** section, do the following: 

   1. For **Timezone**, choose a timezone\. 

   1. For **Start date and time**, enter a valid date in `YYYY/MM/DD` format, then specify a timestamp in 24\-hour `hh:mm` format\. 

   1. For **End date and time**, enter a valid date in `YYYY/MM/DD` format, then specify a timestamp in 24\-hour `hh:mm` format\. 

1. Choose **Next**\. 

1. On the **Select target** page, choose the AWS API operation that EventBridge Scheduler invokes: 

   1. Choose **All APIs**, and then in the search box enter **ECS**\. 

   1. Select **Amazon ECS**\.

   1. In the search box, enter **RunTask**, and then choose **RunTask**\.

   1. For **ECS cluster**, choose the cluster\.

   1. For **ECS task**, choose the task definition to use for the task\.

   1. To use a launch type, expand **Compute options**, and then select **Launch type**\. Then, choose the launch type\.

      When the Fargate launch type is specified, for **Platform version**, enter the platform version to use\. If there is no platform specified, the `LATEST` platform version is used\.

   1. For **Subnets**, enter the subnet IDs to run the task in\.

   1. For **Security groups**, enter the security group IDs for the subnet\.

   1. \(Optional\) To use a task placement strategy other than the default, expand **Placement constraint**, and then enter the constraints\.

       For more information, see [Amazon ECS task placement](task-placement.md)\.

   1. \(Optional\) To help identify your tasks, under **Tags** configure your tags\.

      To have Amazon ECS automatically tag all newly launched tasks with the task definition tags, select **Enable Amazon ECS managed tags**\.

1. Choose **Next**\. 

1. On the **Settings** page, do the following: 

   1. To turn on the schedule, under **Schedule state**, toggle **Enable schedule**\. 

   1. To configure a retry policy for your schedule, under **Retry policy and dead\-letter queue \(DLQ\)**, do the following:
      + Toggle **Retry**\.
      + For **Maximum retention time of event**, enter the maximum **hour\(s\)** and **min\(s\)** that EventBridge Scheduler must keep an unprocessed event\.
      + The maximum is 24 hours\.
      + For **Maximum retries**, enter the maximum number of times EventBridge Scheduler retries the schedule if the target returns an error\. 

         The maximum value is 185 retries\. 

      With retry policies, if a schedule fails to invoke its target, EventBridge Scheduler re\-runs the schedule\. If configured, you must set the maximum retention time and retries for the schedule\.

   1. Choose where EventBridge Scheduler stores undelivered events\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduled_tasks.html)

   1. To use a customer managed KMS key to encrypt your target input, under **Encryption**, choose **Customize encryption settings \(advanced\)** \. 

      If you choose this option, enter an existing KMS key ARN or choose **Create an AWS KMS key** to navigate to the AWS KMS console\. For more information about how EventBridge Scheduler encrypts your data at rest, see [Encryption at rest](https://docs.aws.amazon.com/scheduler/latest/UserGuide/encryption-rest.html) in the *Amazon EventBridge Scheduler User Guide*\. 

   1. For **Permissions**, choose **Use existing role**, then select the role\.

      To have EventBridge Scheduler create a new execution role for you, choose **Create new role for this schedule**\. Then, enter a name for **Role name**\. If you choose this option, EventBridge Scheduler attaches the required permissions necessary for your templated target to the role\. 

1. Choose **Next**\. 

1.  In the **Review and create schedule** page, review the details of your schedule\. In each section, choose **Edit** to go back to that step and edit its details\. 

1. Choose **Create schedule**\. 

   You can view a list of your new and existing schedules on the **Schedules** page\. Under the **Status** column, verify that your new schedule is **Enabled**\. 

## View your scheduled tasks in the console<a name="scheduled-task-view"></a>

Your scheduled tasks can be viewed in the Amazon ECS console\. You can also view the Amazon EventBridge Scheduler scheduler that start the scheduled tasks in the EventBridge Scheduler console\.

**To view your scheduled tasks \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. Choose **Clusters**, and then choose the cluster your scheduled tasks are run in\.

1. On the **Cluster: *cluster\-name*** page, choose the **Scheduled Tasks** tab\.

1. All of your scheduled tasks are listed\.

## Edit a scheduled task<a name="scheduled-task-edit"></a>

You can edit your scheduled tasks in the classic Amazon ECS console\. You can also edit the Amazon EventBridge Scheduler scheduler that start the scheduled tasks in the EventBridge Scheduler console\.

**To edit a scheduled task \(Amazon ECS console\)**

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster in which to edit your scheduled task\.

1. On the **Cluster: *cluster\-name*** page, choose **Scheduled Tasks**\.

1. Select the box to the left of the schedule rule to edit, and choose **Edit**\.

1. Edit the fields to update and choose **Update**\.