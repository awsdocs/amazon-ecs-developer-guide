# Scheduled tasks<a name="scheduled_tasks"></a>

Amazon ECS supports creating scheduled tasks\. Scheduled tasks use Amazon EventBridge rules to run tasks either on a schedule or in a response to an EventBridge event\.

If you want to run tasks at set intervals, such as a backup operation or a log scan, you can create a scheduled task that runs one or more tasks at specified times\. You can specify a regular interval \(run every *N* minutes, hours, or days\), or for more complicated scheduling, you can use a `cron` expression\. For more information, see [Cron expressions and rate expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html) in the *Amazon EventBridge User Guide*\.

If you want to run tasks that are triggered by an event, there are AWS managed events for services \(for example Amazon ECS task and container instance state change events\) or you can create a custom event pattern\. For more information, see [Event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) in the *Amazon EventBridge User Guide*\.

The EventBridge documentation includes a tutorial for creating a scheduled task based on a file being uploaded to an Amazon S3 bucket\. For more information, see [Tutorial: Run an Amazon ECS task when a file is uploaded to an Amazon S3 bucket](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-ecs-tutorial.html) in the *Amazon EventBridge User Guide*\.

**Topics**
+ [Create a scheduled task](#scheduled-task-create)
+ [View your scheduled tasks](#scheduled-task-view)
+ [Edit a scheduled task](#scheduled-task-edit)

## Create a scheduled task<a name="scheduled-task-create"></a>

Scheduled tasks are triggered by Amazon EventBridge rules, which you can create using the EventBridge console\. Although you can create a scheduled task in the Amazon ECS console, currently the EventBridge console provides more functionality so the following steps walk you through creating an EventBridge rule that triggers a scheduled task\.

**Create a scheduled task \(EventBridge console\)**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, **Create rule**\.

1. Enter a name and description for the rule\.
**Note**  
A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For a scheduled task that runs on a schedule, do the following\. To create a scheduled task that runs based on an event, skip to the next step\.

   1. For **Define pattern**, choose **Schedule**\.

   1. Either choose **Fixed rate of** and specify how often the task is to run, or choose **Cron expression** and specify a cron expression that defines when the task is to be triggered\.

   1. For **Select event bus**, choose **AWS default event bus**\. You can only create scheduled rules on the default event bus\.

1. For a scheduled task that runs based on an event, do the following\. If you created your rule based on a schedule, skip to the next step\.

   1. For **Define pattern**, choose **Event pattern**\.

   1. Choose **Pre\-defined pattern by service**\.

   1. For **Service provider**, choose **AWS**\.

   1. For **Service name**, choose the name of the service that emits the event\.

   1. For **Event type**, choose **All Events** or choose the type of event to use for this rule\. If you choose **All Events**, all events emitted by this AWS service will match the rule\.

      To customize the event pattern, choose **Edit**, make your changes, and then choose **Save**\.

   1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to match events that come from your account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\.

1. For **Select targets**, choose **ECS task**\.

1. For **Cluster**, select an Amazon ECS cluster\.

1. For **Task definition**, select a task definition family\.

1. For **Task definition revision**, select either **Latest** or **Revision** and select a specific task definition revision to use\.

1. For **Count**, specify the desired number of tasks to run\.

1. The **Compute options** section can be expanded to change the default compute options for the scheduled task\.

   1. Choose whether your scheduled task will use a **Capacity provider strategy** or **Launch type**\.

   1. To use a capacity provider strategy, choose **Use cluster default** to use the cluster's default capacity provider strategy\. If your cluster doesn't have a default capacity provider strategy, or to use a custom strategy, choose **Use custom**, **Add capacity provider strategy** and define your custom capacity provider strategy by specifying a **Capacity provider**, **Base**, and **Weight**\. In order for a capacity provider to be used in a strategy, it must be associated with the cluster\. For more information about capacity provider strategies, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.

   1. To use a launch type instead, specify **Launch type**, choose the launch type to use\.

   1. \(Optional\) When the Fargate launch type is specified, for **Platform version**, specify the platform version to use\. If a platform version isn't specified, the `LATEST` platform version is used by default\.

1. \(Optional\) Expand **Configure network configuration** to specify a network configuration\. This is required for tasks hosted on Fargate and for tasks using the `awsvpc` network mode\.

   1. For **Subnets**, specify one or more subnet IDs\.

   1. For **Security groups**, specify one or more security group IDs\.

   1. For **Auto\-assign public IP**, specify whether to assign a public IP address from your subnet to the task\.

1. \(Optional\) Expand **Configure additional properties** to specify the following additional parameters for your tasks\.

   1. For **Task group**, specify a task group name\. The task group name is used to identify a set of related tasks and is used in conjunction with the `spread` task placement strategy to ensure tasks in the same task group are spread out evently among the container instances in the cluster\.

   1. For **Placement constraint**, choose **Add placement constraint**\. Select the **Type** for the placement constraint and then enter an expression\. For more information, see [Amazon ECS task placement constraints](task-placement-constraints.md)\.
**Note**  
Task placement constraints aren't supported for tasks hosted on Fargate\.

   1. For **Placement strategy**, choose **Add placement strategy**\. Select the **Type** for the placement strategy and then enter an expression\. Repeat this process for each placement strategy to add\. For more information, see [Amazon ECS task placement strategies](task-placement-strategies.md)\.
**Note**  
Task placement strategies aren't supported for tasks hosted on Fargate\.

   1. For **Tags**, choose **Add tag** to associate key value pair tags for the task\.

   1. For **Configure managed tags**, choose **Enable managed tags** to have Amazon ECS add tags that can be used when reviewing cost allocation in your Cost and Usage Report\. For more information, see [Tagging your resources for billing](ecs-using-tags.md#tag-resources-for-billing)\.

   1. For **Configure execute command**, choose **Enable execute command** to enable the ECS Exec functionality for the task\. For more information, see [Using Amazon ECS Exec for debugging](ecs-exec.md)\.

   1. For **Configure propagate tags**, choose **Propagate tags from task definition** to have Amazon ECS add the tags associated with the task definition to your task\. For more information, see [Tagging your resources](ecs-using-tags.md#tag-resources)\.
**Note**  
If you specify a tag with the same key in the **Tags** section, it will override the tag propagated from the task definition\.

1. For many target types, EventBridge needs permissions to send events to the target\. In these cases, EventBridge can create the IAM role needed for your rule to run\.
   + To create an IAM role automatically, choose **Create a new role for this specific resource**
   + To use an IAM role that you created earlier, choose **Use existing role**

1. For **Retry policy and dead\-letter queue:**, under **Retry policy**:

   1. For **Maximum age of event**, enter a value between one minute \(`00:01`\) and 24 hours \(`24:00`\)\.

   1. For **Retry attempts**, enter a number between 0 and 185\.

1. For **Dead\-letter queue**, choose whether to use a standard Amazon SQS queue as a dead\-letter queue\. EventBridge sends events that match this rule to the dead\-letter queue if they are not successfully delivered to the target\. Do one of the following:
   + Choose **None** to not use a dead\-letter queue\.
   + Choose **Select an Amazon SQS queue in the current AWS account to use as the dead\-letter queue** and then select the queue to use from the drop\-down list\.
   + Choose **Select an Amazon SQS queue in an other AWS account as a dead\-letter queue** and then enter the ARN of the queue to use\. You must attach a resource\-based policy to the queue that grants EventBridge permission to send messages to it\. For more information, see [Granting permissions to the dead\-letter queue](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rule-dlq.html#eb-dlq-perms) in the *Amazon EventBridge User Guide*\.

## View your scheduled tasks<a name="scheduled-task-view"></a>

Your scheduled tasks can be viewed in the Amazon ECS console\. You can also view the Amazon EventBridge rules that trigger the scheduled tasks in the EventBridge console\.

**To view your scheduled tasks \(Amazon ECS console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster your scheduled tasks are run in\.

1. On the **Cluster: *cluster\-name*** page, choose the **Scheduled Tasks** tab\.

1. All of your scheduled tasks are listed\.

## Edit a scheduled task<a name="scheduled-task-edit"></a>

Your scheduled tasks can be edited in the Amazon ECS console\. You can also edit the Amazon EventBridge rules that trigger the scheduled tasks in the EventBridge console\.

**To edit a scheduled task \(Amazon ECS console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster in which to edit your scheduled task\.

1. On the **Cluster: *cluster\-name*** page, choose **Scheduled Tasks**\.

1. Select the box to the left of the schedule rule to edit, and choose **Edit**\.

1. Edit the fields to update and choose **Update**\.