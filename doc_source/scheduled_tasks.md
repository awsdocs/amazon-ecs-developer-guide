# Scheduled Tasks \(`cron`\)<a name="scheduled_tasks"></a>

You can run Amazon ECS tasks on a `cron`\-like schedule using CloudWatch Events rules and targets\.

If you have tasks to run at set intervals in your cluster, such as a backup operation or a log scan, you can use the Amazon ECS console to create a CloudWatch Events rule that runs one or more tasks in your cluster at the specified times\. Your scheduled event rule can be set to either a specific interval \(run every *N* minutes, hours, or days\), or for more complicated scheduling, you can use a `cron` expression\. For more information, see [Schedule Expressions for Rules](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

**Creating a scheduled task**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster in which to create your scheduled task\.

1. On the **Cluster: *cluster\-name*** page, choose **Scheduled Tasks**, **Create**\.

1. For **Schedule rule name**, enter a unique name for your schedule rule\. Up to 64 letters, numbers, periods, hyphens, and underscores are allowed\.

1. \(Optional\) For **Schedule rule description**, enter a description for your rule\. Up to 512 characters are allowed\.

1. For **Schedule rule type**, choose whether to use a fixed interval schedule or a `cron` expression for your schedule rule\. For more information, see [Schedule Expressions for Rules](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

   + For **Run at fixed interval**, enter the interval and unit for your schedule\.

   + For **Cron expression**, enter the `cron` expression for your task schedule\. These expressions have six required fields, and fields are separated by white space\. For more information, and examples of `cron` expressions, see [Cron Expressions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) in the *Amazon CloudWatch Events User Guide*\.

1. Create a target for your schedule rule\.

   1. For **Target ID**, enter a unique identifier for your target\. Up to 64 letters, numbers, periods, hyphens, and underscores are allowed\.

   1. For **Task definition**, choose the family and revision \(family:revision\) of the task definition to run for this target\.

   1. For **Number of tasks**, enter the number of instantiations of the specified task definition to run on your cluster when the rule executes\.

   1. \(Optional\) For **Task role override**, choose the IAM role to use for the task in your target, instead of the task definition default\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\. Only roles with the **Amazon EC2 Container Service Task Role** trust relationship are shown here\. For more information about creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

   1. For **CloudWatch Events IAM role for this target**, choose an existing CloudWatch Events service role \(`ecsEventsRole`\) that you may have already created\. Or, choose **Create new role** to create the required IAM role that allows CloudWatch Events to make calls to Amazon ECS to run tasks on your behalf\. For more information, see [CloudWatch Events IAM Role](CWE_IAM_role.md)\.

   1. \(Optional\) In the **Container overrides** section, you can expand individual containers and override the command and/or environment variables for that container that are defined in the task definition\.

1. \(Optional\) To add additional targets \(other tasks to run when this rule is executed\), choose **Add targets** and repeat the previous substeps for each additional target\.

1. Choose **Create**\.

**To edit a scheduled task**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster in which to edit your scheduled task\.

1. On the **Cluster: *cluster\-name*** page, choose **Scheduled Tasks**\.

1. Select the box to the left of the schedule rule to edit, and choose **Edit**\.

1. Edit the fields to update and choose **Update**\.