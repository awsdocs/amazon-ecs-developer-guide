# Checking stopped tasks for errors<a name="stopped-task-errors"></a>

If you have trouble starting a task, your task might be stopping because of application or configuration errors\. For example, you run the task and the task displays a `PENDING` status and then disappears\. You can view stopped task errors like this in the Amazon ECS console by viewing the stopped task and inspecting it for error messages\.

If your task definition uses the `awslogs` log driver, the application logs that are written to Amazon CloudWatch Logs are displayed on the **Logs** tab in the Amazon ECS console as long as the stopped task appears\.

If your task was created by an Amazon ECS service, the actions that Amazon ECS takes to maintain the service are published in the service events\. You can view the events in the AWS Management Console, AWS CLI, AWS SDKs, the Amazon ECS API, or tools that use the SDKs and API\. These events include Amazon ECS stopping and replaces a task because the containers in the task have stopped running, or have failed too many health checks from Elastic Load Balancing\. For more information, see [Service event messages](service-event-messages.md)\.

If your task ran on a container instance on Amazon EC2 or external computers, you can also look at the logs of the container runtime and the ECS Agent\. These logs are on the host EC2 instance or external computer\. For more information, see [Amazon ECS Log File Locations](logs.md)\.

**Important**  
Stopped tasks only appear in the Amazon ECS console, AWS CLI, and AWS SDKs for at least 1 hour after the task stops\. After that, the details of the stopped task expire and aren't available in Amazon ECS\.  
Amazon ECS also sends task state change events to Amazon EventBridge\. You can't view events in EventBridge\. Instead, you create rules to send the events to other persistent storage such as Amazon CloudWatch Logs\. You can use the storage to view your stopped task details after it has expired from view in the Amazon ECS console\. For more information, see [Task state change events](ecs_cwe_events.md#ecs_task_events)\.  
For a sample EventBridge configuration to archive Amazon ECS events to Amazon CloudWatch Logs, see [ECS Stopped Tasks in CloudWatch Logs](https://github.com/aws-samples/amazon-ecs-stopped-tasks-cwlogs#ecs-stopped-tasks-in-cloudwatch-logs) on the GitHub website\.

Follow these steps to check stopped tasks for errors\.

------
#### [ Console ]

**AWS Management Console**

The following steps can be used to check stopped tasks for errors using the new AWS Management Console\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose the **Tasks** tab\. 

1. Choose the stopped task to inspect\.

1. In the **Status** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.

------
#### [ AWS CLI ]

1. List the stopped tasks in a cluster\. The output contains the Amazon Resource Name \(ARN\) of the task, which you need to describe the task\.

   ```
   aws ecs list-tasks \
        --cluster cluster_name \
        --desired-status STOPPED \
        --region us-west-2
   ```

1. Describe the stopped task to retrieve the `stoppedReason` in the response\.

   ```
   aws ecs describe-tasks \
        --cluster cluster_name \
        --tasks arn:aws:ecs:us-west-2:account_id:task/cluster_name/task_ID \
        --region us-west-2
   ```

------

## Additional resources<a name="additional-resources"></a>

The following pages provide additional information about error codes:
+ [Why is my Amazon ECS task stopped](https://aws.amazon.com/premiumsupport/knowledge-center/ecs-task-stopped/)
+  [Stopped tasks error codes](https://docs.aws.amazon.com/AmazonECS/latest/userguide/stopped-task-error-codes.html)