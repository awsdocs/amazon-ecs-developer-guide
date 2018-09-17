# Updating a Service<a name="update-service"></a>

You can update a running service to change the number of tasks that are maintained by a service, which task definition is used by the tasks, or if you are using Fargate tasks you can change the platform version your service uses\. If you have an application that needs more capacity, you can scale up your service\. If you have unused capacity that you would like to scale down, you can reduce the number of desired tasks in your service and free up resources\.

If you have updated the Docker image of your application, you can create a new task definition with that image and deploy it to your service\. The service scheduler uses the minimum healthy percent and maximum percent parameters \(in the service's deployment configuration\) to determine the deployment strategy\.

**Note**  
If your updated Docker image uses the same tag as what is in the existing task definition for your service \(for example, `my_image:latest`\), you do not need to create a new revision of your task definition\. You can update the service using the procedure below, keep the current settings for your service, and select **Force new deployment**\. The new tasks launched by the deployment pull the current image/tag combination from your repository when they start\. The **Force new deployment** option is also used when updating a Fargate task to use a more current platform version when you specify `LATEST`\. For example, if you specified `LATEST` and your running tasks are using the `1.0.0` platform version and you want them to relaunch using a newer platform version\.

The minimum healthy percent represents a lower limit on the number of your service's tasks that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. This parameter enables you to deploy without using additional cluster capacity\. For example, if your service has a desired number of four tasks and a minimum healthy percent of 50%, the scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\. Tasks for services that *do not* use a load balancer are considered healthy if they are in the `RUNNING` state; tasks for services that *do* use a load balancer are considered healthy if they are in the `RUNNING` state and the container instance on which it is hosted is reported as healthy by the load balancer\. The default value for minimum healthy percent is 50% in the console and 100% for the AWS CLI, the AWS SDKs, and the APIs\.

The maximum percent parameter represents an upper limit on the number of your service's tasks that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\. This parameter enables you to define the deployment batch size\. For example, if your service has a desired number of four tasks and a maximum percent value of 200%, the scheduler may start four new tasks before stopping the four older tasks \(provided that the cluster resources required to do this are available\)\. The default value for maximum percent is 200%\.

When the service scheduler replaces a task during an update, if a load balancer is used by the service, the service first removes the task from the load balancer and waits for the connections to drain\. Then the equivalent of docker stop is issued to the containers running in the task\. This results in a `SIGTERM` signal and a 30\-second timeout, after which `SIGKILL` is sent and the containers are forcibly stopped\. If the container handles the `SIGTERM` signal gracefully and exits within 30 seconds from receiving it, no `SIGKILL` signal is sent\. The service scheduler starts and stops tasks as defined by your minimum healthy percent and maximum percent settings\. 

**Important**  
If you are changing the ports used by containers in a task definition, you may need to update your container instance security groups to work with the updated ports\.  
If your service uses a load balancer, the load balancer configuration defined for your service when it was created cannot be changed\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.  
To change the load balancer name, the container name, or the container port associated with a service load balancer configuration, you must create a new service\.  
Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To update a running service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster in which your service resides\.

1. On the **Cluster: *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Update**\.

1. On the **Configure service** page, your service information is pre\-populated\. Change the task definition, platform version, deployment configuration, or number of desired tasks \(or any combination of these\) and choose **Next step**\.
**Note**  
If you want your service to use a newly updated Docker image with the same tag as what is in the existing task definition \(for example, `my_image:latest`\), or keep the current settings for your service, select **Force new deployment**\. The new tasks launched by the deployment pull the current image/tag combination from your repository when they start\. The **Force new deployment** option is also used when updating a Fargate task to use a more current platform version when you specify `LATEST`\. For example, if you specified `LATEST` and your running tasks are using the `1.0.0` platform version and you want them to relaunch using a newer platform version\.

1. On the **Configure network** page, your network information is pre\-populated\. Change the health check grace period \(if desired\) and choose **Next step**\.

1. \(Optional\) You can use Service Auto Scaling to scale your service up and down automatically in response to CloudWatch alarms\. 

   1. Under **Optional configurations**, choose **Configure Service Auto Scaling**\.

   1. Proceed to [\(Optional\) Configuring Your Service to Use Service Auto Scaling](create-service.md#service-configure-auto-scaling)\.

   1. Complete the steps in that section and then return here\.

1. Choose **Update Service** to finish and update your service\.