# Updating a service<a name="update-service"></a>

You can update an existing service to change some of the service configuration parameters, such as the number of tasks that are maintained by a service, which task definition is used by the tasks, or if your tasks are using the Fargate launch type, you can change the platform version your service uses\. If you have an application that needs more capacity, you can scale up your service\. If you have unused capacity to scale down, you can reduce the number of desired tasks in your service and free up resources\.

If you want to use an updated container image for your tasks, you can create a new task definition revision with that image and deploy it to your service by using the **force new deployment** option in the console\.

The service scheduler uses the minimum healthy percent and maximum percent parameters \(in the deployment configuration for the service\) to determine the deployment strategy\.

If a service is using the rolling update \(`ECS`\) deployment type, the **minimum healthy percent** represents a lower limit on the number of tasks in a service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. The parameter also applies while any container instances are in the `DRAINING` state if the service contains tasks using the EC2 launch type\. This parameter enables you to deploy without using additional cluster capacity\. For example, if your service has a desired number of four tasks and a minimum healthy percent of 50%, the scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\. Tasks for services that do not use a load balancer are considered healthy if they are in the `RUNNING` state\. Tasks for services that do use a load balancer are considered healthy if they are in the `RUNNING` state and they are reported as healthy by the load balancer\. The default value for minimum healthy percent is 100%\.

If a service is using the rolling update \(`ECS`\) deployment type, the **maximum percent** parameter represents an upper limit on the number of tasks in a service that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\. The parameter also applies while any container instances are in the `DRAINING` state if the service contains tasks using the EC2 launch type\. This parameter enables you to define the deployment batch size\. For example, if your service has a desired number of four tasks and a maximum percent value of 200%, the scheduler may start four new tasks before stopping the four older tasks\. That's provided that the cluster resources required to do this are available\. The default value for the maximum percent is 200%\.

If a service is using the blue/green \(`CODE_DEPLOY`\) deployment type and tasks that use the EC2 launch type, the **minimum healthy percent** and **maximum percent** values are set to the default values\. They are only used to define the lower and upper limit on the number of the tasks in the service that remain in the `RUNNING` state while the container instances are in the `DRAINING` state\. If the tasks in the service use the Fargate launch type, the minimum healthy percent and maximum percent values are not used\. They are currently visible when describing your service\.

When the service scheduler replaces a task during an update, the service first removes the task from the load balancer \(if used\) and waits for the connections to drain\. Then, the equivalent of docker stop is issued to the containers running in the task\. This results in a `SIGTERM` signal and a 30\-second timeout, after which `SIGKILL` is sent and the containers are forcibly stopped\. If the container handles the `SIGTERM` signal gracefully and exits within 30 seconds from receiving it, no `SIGKILL` signal is sent\. The service scheduler starts and stops tasks as defined by your minimum healthy percent and maximum percent settings\. 

**Important**  
If you are changing the ports used by containers in a task definition, you may need to update the security groups for the container instances to work with the updated ports\.  
If your service uses a load balancer, the load balancer configuration defined for your service when it was created cannot be changed\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.  
To change the load balancer name, the container name, or the container port associated with a service load balancer configuration, you must create a new service\.  
Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To update a running service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the Region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster in which your service resides\.

1. On the **Cluster: *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Update**\.

1. On the **Configure service** page, your service information is pre\-populated\. Change the task definition, capacity provider strategy, platform version, deployment configuration, or number of desired tasks \(or any combination of these\)\. To have your service start a new deployment, which will stop and relaunch all tasks using the new configuration, select **Force new deployment**\. Choose **Next step** when finished changing the service configuration\.

1. On the **Configure deployments** page, if your service is using the blue/green deployment type, the components of your service deployment is pre\-populated\. Confirm the following settings\.

   1. For **Application name**, choose the CodeDeploy application of which your service is a part\.

   1. For **Deployment group name**, choose the CodeDeploy deployment group of which your service is a part\.

   1. Select the deployment lifecycle event hooks and the associated Lambda functions to execute as part of the new revision of the service deployment\. The available lifecycle hooks are:
      + **BeforeInstall** – Use this deployment lifecycle event hook to invoke a Lambda function before the replacement task set is created\. The result of the Lambda function at this lifecycle event does not trigger a rollback\.
      + **AfterInstall** – Use this deployment lifecycle event hook to invoke a Lambda function after the replacement task set is created\. The result of the Lambda function at this lifecycle event can trigger a rollback\.
      + **BeforeAllowTraffic** – Use this deployment lifecycle event hook to invoke a Lambda function before the production traffic has been rerouted to the replacement task set\. The result of the Lambda function at this lifecycle event can trigger a rollback\.
      + **AfterAllowTraffic** – Use this deployment lifecycle event hook to invoke a Lambda function after the production traffic has been rerouted to the replacement task set\. The result of the Lambda function at this lifecycle event can trigger a rollback\.

      For more information about lifecycle hooks, see [AppSpec 'hooks' Section](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html) in the *AWS CodeDeploy User Guide*\.

1. Choose **Next step**\.

1. On the **Configure network** page, your network information is pre\-populated\. In the **Load balancing** section, if your service is using the blue/green deployment type, select the listeners to associate with the target groups\. Change the health check grace period \(if desired\) and choose **Next step**\.

1. \(Optional\) You can use Service Auto Scaling to scale your service up and down automatically in response to CloudWatch alarms\. 

   1. Under **Optional configurations**, choose **Configure Service Auto Scaling**\.

   1. Proceed to [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)\.

   1. Complete the steps in that section and then return\.

1. Choose **Update Service** to finish and update your service\.