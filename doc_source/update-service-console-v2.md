# Updating a service using the console<a name="update-service-console-v2"></a>

You can update an Amazon ECS service using the Amazon ECS experience\. The current service configuration is pre\-populated\. You are able to update the task definition, desired task count, capacity provider strategy, platform version, and deployment configuration; or any combination of these\.

Consider the following when you use the new console:
+ Only services using the **Rolling update** \(`ECS`\) deployment type can be updated using the new Amazon ECS experience\.
+ Currently, the console supports only the **Target tracking** scaling policy\. To use step scaling, switch to the classic console\.
+ Currently, the console supports only the **Replica** service type\. To use the Daemon service type, switch to the classic console\.
+ If you are changing the ports used by containers in a task definition, you might need to update the security groups for the container instances to work with the updated ports\.
+ Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.
+ If your service uses a load balancer, the load balancer configuration defined for your service when it was created cannot be changed\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.
+ To change the load balancer name, the container name, or the container port associated with a service load balancer configuration, you must create a new service\.

You can update an existing service to change some of the service configuration parameters, such as the number of tasks that are maintained by a service, which task definition is used by the tasks, or if your tasks are using the Fargate launch type, you can change the platform version your service uses\. A service using a Linux platform version cannot be updated to use a Windows platform version and vice versa\. If you have an application that needs more capacity, you can scale up your service\. If you have unused capacity to scale down, you can reduce the number of desired tasks in your service and free up resources\.

If you want to use an updated container image for your tasks, you can create a new task definition revision with that image and deploy it to your service by using the **force new deployment** option in the console\.

The service scheduler uses the minimum healthy percent and maximum percent parameters \(in the deployment configuration for the service\) to determine the deployment strategy\.

If a service is using the rolling update \(`ECS`\) deployment type, the **minimum healthy percent** represents a lower limit on the number of tasks in a service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. The parameter also applies while any container instances are in the `DRAINING` state if the service contains tasks using the EC2 launch type\. Use this parameter to deploy without using additional cluster capacity\. For example, if your service has a desired number of four tasks and a minimum healthy percent of 50 percent, the scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\. Tasks for services that do not use a load balancer are considered healthy if they are in the `RUNNING` state\. Tasks for services that do use a load balancer are considered healthy if they are in the `RUNNING` state and they are reported as healthy by the load balancer\. The default value for minimum healthy percent is 100 percent\.

If a service is using the rolling update \(`ECS`\) deployment type, the **maximum percent** parameter represents an upper limit on the number of tasks in a service that are allowed in the `PENDING`, `RUNNING`, or `STOPPING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\. The parameter also applies while any container instances are in the `DRAINING` state if the service contains tasks using the EC2 launch type\. Use this parameter to define the deployment batch size\. For example, if your service has a desired number of four tasks and a maximum percent value of 200 percent, the scheduler may start four new tasks before stopping the four older tasks\. That is provided that the cluster resources required to do this are available\. The default value for the maximum percent is 200 percent\.

When the service scheduler replaces a task during an update, the service first removes the task from the load balancer \(if used\) and waits for the connections to drain\. Then, the equivalent of docker stop is issued to the containers running in the task\. This results in a `SIGTERM` signal and a 30\-second timeout, after which `SIGKILL` is sent and the containers are forcibly stopped\. If the container handles the `SIGTERM` signal gracefully and exits within 30 seconds from receiving it, no `SIGKILL` signal is sent\. The service scheduler starts and stops tasks as defined by your minimum healthy percent and maximum percent settings\. 

**Important**  
If you are changing the ports used by containers in a task definition, you may need to update the security groups for the container instances to work with the updated ports\.  
If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.  
To change the container name, or the container port associated with a service load balancer configuration, you must create a new service\.  
Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To update a service \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster\.

1. On the **Cluster overview** page, select the service, and then choose **Update**\.

1. To have your service start a new deployment, select **Force new deployment**\.

1. For **Task definition**, choose the task definition family and revision\.
**Important**  
The console validates that the selected task definition family and revision is compatible with the defined compute configuration\. If you receive a warning, verify both your task definition compatibility and the compute configuration selected\.

1. For **Desired tasks**, enter the number of tasks you want to run for the service\.

1. For **Min running tasks**, enter the lower limit on the number of tasks in the service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. For more information, see [Deployment configuration](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service_definition_parameters.html#sd-deploymentconfiguration)\.

1. For **Max running tasks**, enter the upper limit on the number of tasks in the service that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\.

1. To configure how Amazon ECS detects and handles deployment failures, expand **Deployment failure detection**, and then choose your options\. 

   1. To stop a deployment when the tasks cannot start, select **Use the Amazon ECS deployment circuit breaker**\.

      To have the software automatically roll back the deployment to the last completed deployment state when the deployment circuit breaker sets the deployment to a failed state, select **Rollback on failure**\.

   1. To stop a deployment based on application metrics\., select **Use CloudWatch alarms**\. Then, from **CloudWatch alarm names**, choose the alarms\. To create a new alarm, choose **Create new alarm**\.

      To have the software automatically roll back the deployment to the last completed deployment state when a CloudWatch alarm sets the deployment to a failed state, select **Rollback on failure**\.

1. To change the compute options, expand **Deployment options**, **Compute configuration**, and then do the following: 

   1. For services on AWS Fargate, for **Platform version**, choose the new version\.

   1. For services that use a capacity provider strategy, for **capacity provider strategy**, do the following:
      + To add an additional capacity provider, choose **Add more**\. Then, for **Capacity provider**, choose the capacity provider\.
      + To remove a capacity provider, to the right of the capacity provider, choose **Remove**\.

      A service using an Auto Scaling group capacity provider can't be updated to use a Fargate capacity provider and vice versa\.

1. To configure service auto scaling, expand **Service auto scaling**, and then specify the following parameters\.

   1. To use service auto scaling, select **Service auto scaling**\.

   1. For **Minimum number of tasks**, enter the lower limit of the number of tasks for Service Auto Scaling to use\. The desired count will not go below this count\.

   1. For **Maximum number of tasks**, enter the upper limit of the number of tasks for Service Auto Scaling to use\. The desired count will not go above this count\.

   1. For **Scaling policy type**, choose **Target tracking**\.

   1. For **Policy name**, enter the policy name\.

   1. For **ECS service metric**, choose one of the following metrics:
      + **ECSServiceAverageCPUUtilization**: Average CPU utilization of the service\. 
      + **ECSServiceAverageMemoryUtilization**: Average memory utilization of the service\. 
      + **ALBRequestCountPerTarget**: Number of requests completed per target in an Application Load Balancer target group\. 

        The metrics requires an Application Load Balancer and a target group for the Application Load Balancer\.

   1. For **Target value**, enter the value in percent that the service maintains for the selected metric\.

   1. For **Scale\-out cooldown period**, enter the number of seconds after a scale\-out activity that no other scale outs can happen\.

   1. For **Scale\-in cooldown period**, enter the number of seconds after a scale\-in activity that no other scale ins can happen\.

   1. To prevent the policy from performing a scale\-in activity, select **Turn off scale\-in**\.

1. \(Optional\) To help identify your service, expand the **Tags** section, and then configure your tags\.
   + \[Add a tag\] Choose **Add tag** and do the following:
     + For **Key**, enter the key name\.
     + For **Value**, enter the key value\.
   + \[Remove a tag\] Next to the tag, choose **Remove tag**\.

1. Choose **Update**\.