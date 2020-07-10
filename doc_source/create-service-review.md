# Step 6: Review and create your service<a name="create-service-review"></a>

After you have configured your basic service definition parameters and optionally configured your service's networking, load balancer, service discovery, and automatic scaling, you can review your configuration\. Then, choose **Create Service** to finish creating your service\.

**Note**  
After you create a service, the target group ARN or load balancer name, container name, and container port specified in the service definition are immutable\. You cannot add, remove, or change the load balancer configuration of an existing service\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.