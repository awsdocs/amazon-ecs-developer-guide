# Checking Stopped Tasks for Errors<a name="stopped-task-errors"></a>

If you have trouble starting a task \(for example, you run the task and the task displays a `PENDING` status and then disappears\), your task might be stopping because of an error\. You can view errors like this in the Amazon ECS console by displaying the stopped task and inspecting it for error messages\.

**To check stopped tasks for errors**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, choose the cluster in which your stopped task resides\.

1. On the **Cluster : *clustername*** page, choose**Tasks**\.

1. In the **Desired task status** table header, choose **Stopped** to view stopped tasks, and then choose the stopped task to inspect\. The most recent stopped tasks are listed first\.

1. In the **Details** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.  
![\[Stopped task reason\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_task_reason.png)

   Some possible reasons and their explanations are listed below:  
Task failed ELB health checks in \(elb elb\-name\)  
The current task failed the Elastic Load Balancing health check for the load balancer that is associated with the task's service\. For more information, see [Troubleshooting Service Load Balancers](troubleshoot-service-load-balancers.md)\.  
Scaling activity initiated by \(deployment deployment\-id\)  
When you reduce the desired count of a stable service, some tasks need to be stopped in order to reach the desired number\. Tasks that are stopped by downscaling services have this stopped reason\.   
Host EC2 \(instance *id*\) stopped/terminated  
If you stop or terminate a container instance with running tasks, then the tasks are given this stopped reason\.  
Container instance deregistration forced by user  
If you force the deregistration of a container instance with running tasks, then the tasks are given this stopped reason\.  
Essential container in task exited  
Containers marked as `essential` in task definitions cause a task to stop if they exit or die\. When an essential container exiting is the cause of a stopped task, the [[ERROR] BAD/MISSING LINK TEXT](#status-reason-step) can provide more diagnostic information as to why the container stopped\.

1. If you have a container that has stopped, expand the container and inspect the **Status reason** row to see what caused the task state to change\.  
![\[Stopped container error\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_container_status_reason.png)

   In the previous example, the container image name cannot be found\. This can happen if you misspell the image name\.

   If this inspection does not provide enough information, you can connect to the container instance with SSH and inspect the Docker container locally\. For more information, see [Inspect Docker Containers](docker-diags.md#docker-inspect)\.