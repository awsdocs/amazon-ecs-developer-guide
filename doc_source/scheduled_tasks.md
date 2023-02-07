# Scheduled tasks<a name="scheduled_tasks"></a>

Amazon ECS supports creating scheduled tasks\. Scheduled tasks use Amazon EventBridge rules to run tasks either on a schedule or in a response to an EventBridge event\.

If you want to run tasks at set intervals, such as a backup operation or a log scan, you can create a scheduled task that runs one or more tasks at specified times\. You can specify a regular interval \(run every *N* minutes, hours, or days\), or for more complicated scheduling, you can use a `cron` expression\. For more information, see [Cron expressions and rate expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html) in the *Amazon EventBridge User Guide*\.

If you want to run tasks that are started by an event, there are AWS managed events for services \(for example Amazon ECS task and container instance state change events\) or you can create a custom event pattern\. For more information, see [Event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) in the *Amazon EventBridge User Guide*\.

**Topics**
+ [Create a scheduled task](#scheduled-task-create)
+ [View your scheduled tasks in the classic console](#scheduled-task-view)
+ [Edit a scheduled task](#scheduled-task-edit)

## Create a scheduled task<a name="scheduled-task-create"></a>

Scheduled tasks are started by Amazon EventBridge rules, which you can create using the EventBridge console\. Although you can create a scheduled task in the Amazon ECS console, currently the EventBridge console provides more functionality so the following steps walk you through creating an EventBridge rule that starts a scheduled task\.

Before you can submit scheduled tasks with EventBridge rules and targets, the EventBridge service needs several permissions to run Amazon ECS tasks on your behalf\. For more information about the required service principal and IAM permissions for this role, see [Amazon ECS CloudWatch Events IAM Role](CWE_IAM_role.md)\.

**Create a scheduled task \(EventBridge console\)**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.
**Note**  
A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to match events that come from your account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\.

1. Choose how to schedule the task\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduled_tasks.html)

1. Choose **Next**\.

1. For **Target types**, choose **AWS service**\.

1. For **Select a target**, select **ECS task**\.

1. For **Cluster**, select an Amazon ECS cluster\.

1. For **Task definition**, select a task definition family\.

1. For **Task definition revision**, select either **Latest** or **Revision** and select a specific task definition revision to use\.

1. For **Count**, specify the desired number of tasks to run\.

1. Choose how your scheduled task is distributed across your cluster infrastructure\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduled_tasks.html)

1. If the task is hosted on Fargate or uses the `awsvpc` network mode, expand **Configure network configuration** and specify a network configuration\.

   1. For **Subnets**, specify one or more subnet IDs\.

   1. For **Security groups**, specify one or more security group IDs\.

   1. For **Auto\-assign public IP**, specify whether to assign a public IP address from your subnet to the task\.

1. \(Optional\) To specify additional parameters for your tasks, expand **Configure additional properties**\.

   1. For **Task group**, specify a task group name\. The task group name is used to identify a set of related tasks and is used in conjunction with the `spread` task placement strategy to ensure tasks in the same task group are spread out evenly among the container instances in the cluster\.

   1. For **Placement constraint**, choose **Add placement constraint**\. Select the **Type** for the placement constraint and then enter an expression\. For more information, see [Amazon ECS task placement constraints](task-placement-constraints.md)\.
**Note**  
Task placement constraints aren't supported for tasks hosted on Fargate\.

   1. For **Placement strategy**, choose **Add placement strategy**\. Select the **Type** for the placement strategy and then enter an expression\. Repeat this process for each placement strategy to add\. For more information, see [Amazon ECS task placement strategies](task-placement-strategies.md)\.
**Note**  
Task placement strategies aren't supported for tasks hosted on Fargate\.

   1. For **Tags**, choose **Add tag** to associate key value pair tags for the task\.

   1. To add tags to use when reviewing cost allocation in your Cost and Usage Report, for **Configure managed tags**, choose **Enable managed tags**\. For more information, see [Tagging your resources for billing](ecs-using-tags.md#tag-resources-for-billing)\.

   1. To use the ECS Exec functionality for the task, for **Configure execute command**, choose **Enable execute command**\. For more information, see [Using Amazon ECS Exec for debugging](ecs-exec.md)\.

   1. To add the tags associated with the task definition to your task, for **Configure propagate tags**, choose **Propagate tags from task definition**\. For more information, see [How resources are tagged](ecs-using-tags.md#tag-resources)\.
**Note**  
If you specify a tag with the same key in the **Tags** section, that tag overrides the tag that is propagated from the task definition\.

1. For many target types, EventBridge needs permissions to send events to the target\. In these cases, EventBridge can create the IAM role needed for your rule to run\. Do one of the following:
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created earlier, choose **Use existing role** and select the existing role from the dropdown\.

1. \(Optional\) For **Additional settings**, do the following:

   1. For **Maximum age of event**, enter a value between one minute \(00:01\) and 24 hours \(24:00\)\.

   1. For **Retry attempts**, enter a number between 0 and 185\.

   1. For **Dead\-letter queue**, choose whether to use a standard Amazon SQS queue as a dead\-letter queue\. EventBridge sends events that match this rule to the dead\-letter queue if they are not successfully delivered to the target\. Do one of the following:
      + Choose **None** to not use a dead\-letter queue\.
      + Choose **Select an Amazon SQS queue in the current AWS account to use as the dead\-letter queue** and then select the queue to use from the dropdown\.
      + Choose **Select an Amazon SQS queue in an other AWS account as a dead\-letter queue** and then enter the ARN of the queue to use\. You must attach a resource\-based policy to the queue that grants EventBridge permission to send messages to it\. For more information, see [Granting permissions to the dead\-letter queue](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rule-dlq.html#eb-dlq-perms) in the *Amazon EventBridge User Guide*\.

1. Choose **Next**\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Amazon EventBridge tags](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Next**\.

1. Review the details of the rule and choose **Create rule**\.

## View your scheduled tasks in the classic console<a name="scheduled-task-view"></a>

Your scheduled tasks can be viewed in the classic Amazon ECS classic console\. You can also view the Amazon EventBridge rules that start the scheduled tasks in the EventBridge console\.

**To view your scheduled tasks \(Amazon ECS console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster your scheduled tasks are run in\.

1. On the **Cluster: *cluster\-name*** page, choose the **Scheduled Tasks** tab\.

1. All of your scheduled tasks are listed\.

## Edit a scheduled task<a name="scheduled-task-edit"></a>

You can edit your scheduled tasks in the classic Amazon ECS console\. You can also edit the Amazon EventBridge rules that start the scheduled tasks in the EventBridge console\.

**To edit a scheduled task \(Amazon ECS console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster in which to edit your scheduled task\.

1. On the **Cluster: *cluster\-name*** page, choose **Scheduled Tasks**\.

1. Select the box to the left of the schedule rule to edit, and choose **Edit**\.

1. Edit the fields to update and choose **Update**\.