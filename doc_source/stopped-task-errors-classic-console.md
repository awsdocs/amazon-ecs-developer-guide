# Checking stopped tasks for errors in the Amazon ECS classic console<a name="stopped-task-errors-classic-console"></a>

If you have trouble starting a task, your task might be stopping because of an error\. For example, you run the task and the task displays a `PENDING` status and then disappears\. You can view stopped task errors like this in the Amazon ECS console by viewing the stopped task and inspecting it for error messages\. If your task definition uses the `awslogs` log driver, the application logs that are written to Amazon CloudWatch Logs are displayed on the **Logs** tab in the Amazon ECS console as long as the stopped task appears\.

**Important**  
Stopped tasks only appear in the Amazon ECS console, AWS CLI, and AWS SDKs for at least 1 hour after the task stops\. After that, the details of the stopped task expire and aren't available in Amazon ECS\.  
Amazon ECS also sends task state change events to Amazon EventBridge\. You can't view events in EventBridge\. Instead, you create rules to send the events to other persistent storage such as Amazon CloudWatch Logs\. You can use the storage to view your stopped task details after it has expired from view in the Amazon ECS console\. For more information, see [Task state change events](ecs_cwe_events.md#ecs_task_events)\.  
For a sample EventBridge configuration to archive Amazon ECS events to Amazon CloudWatch Logs, see [ECS Stopped Tasks in CloudWatch Logs](https://github.com/aws-samples/amazon-ecs-stopped-tasks-cwlogs#ecs-stopped-tasks-in-cloudwatch-logs) on the GitHub website\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster where your stopped task resides\.

1. On the **Cluster : *clustername*** page, choose **Tasks**\.

1. In the **Desired task status** table header, choose **Stopped**, and then select the stopped task to inspect\. The most recent stopped tasks are listed first\.

1. In the **Details** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.

   Some possible reasons and their explanations are listed below:  
Task failed ELB health checks in \(elb elb\-name\)  
The current task failed the Elastic Load Balancing health check for the load balancer that's associated with the task's service\. For more information, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.  
Scaling activity initiated by \(deployment deployment\-id\)  
When you reduce the desired count of a stable service, some tasks must be stopped to reach the desired number\. Tasks that are stopped by downscaling services have this stopped reason\.   
Host EC2 \(instance *id*\) stopped/terminated  
If you stop or terminate a container instance with running tasks, then the tasks are given this stopped reason\.  
Container instance deregistration forced by user  
If you force the deregistration of a container instance with running tasks, then the tasks are given this stopped reason\.  
Essential container in task exited  
If a container marked as `essential` in task definitions exits or dies, that can cause a task to stop\. When an essential container exiting is the cause of a stopped task, the [Step 6](#status-reason-step) can provide more diagnostic information about why the container stopped\.

1. <a name="status-reason-step"></a>If you have a container that has stopped, expand the container and inspect the **Status reason** row to see what caused the task state to change\.

   If this inspection doesn't provide enough information, you can connect to the container instance with SSH and inspect the Docker container locally\. For more information, see [Inspect Docker Containers](docker-diags.md#docker-inspect)\.