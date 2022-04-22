# Checking stopped tasks for errors<a name="stopped-task-errors"></a>

If you have trouble starting a task, your task might be stopping because of an error\. For example, you run the task and the task displays a `PENDING` status and then disappears\. You can view stopped task errors like this in the Amazon ECS console by viewing the stopped task and inspecting it for error messages\.

**Important**  
Amazon ECS also sends task state change events to EventBridge, which you can view if your stopped task has expired from view in the Amazon ECS console\. For more information, see [Task state change events](ecs_cwe_events.md#ecs_task_events)\.  
For information about how to investigate a task that was stopped more than 1 hour, see [ECS Stopped Tasks in CloudWatch Logs](https://github.com/aws-samples/amazon-ecs-stopped-tasks-cwlogs#ecs-stopped-tasks-in-cloudwatch-logs) on the GitHub website\.

------
#### [ New console ]

**New AWS Management Console**

The following steps can be used to set a container instance to draining using the new AWS Management Console\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose the **Tasks** tab\. 

1. Choose the stopped task to inspect\.

1. In the **Status** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.

------
#### [ Classic console ]

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster where your stopped task resides\.

1. On the **Cluster : *clustername*** page, choose **Tasks**\.

1. In the **Desired task status** table header, choose **Stopped**, and then select the stopped task to inspect\. The most recent stopped tasks are listed first\.

1. In the **Details** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.  
![\[Stopped task reason\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_task_reason.png)

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
![\[Stopped container error\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_container_status_reason.png)

   In the previous example, the container image name can't be found\. This can happen if you misspell the image name\.

   If this inspection doesn't provide enough information, you can connect to the container instance with SSH and inspect the Docker container locally\. For more information, see [Inspect Docker Containers](docker-diags.md#docker-inspect)\.

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