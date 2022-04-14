# Updating a service using the new console<a name="update-service-console-v2"></a>

You can update an Amazon ECS service using the new console\. When updating a service using the AWS Management Console, the current service configuration is pre\-populated\. You are able to update the task definition, desired task count, capacity provider strategy, platform version, and deployment configuration; or any combination of these\.

**Note**  
Currently, only services using the **Rolling update** \(`ECS`\) deployment type should be updated using the new console\. To update a service using any other deployment type, switch to the old console\.  
If you are changing the ports used by containers in a task definition, you may need to update the security groups for the container instances to work with the updated ports\.  
If your service uses a load balancer, the load balancer configuration defined for your service when it was created cannot be changed\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.  
To change the load balancer name, the container name, or the container port associated with a service load balancer configuration, you must create a new service\.  
Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To update a service \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster\.

1. On the **Cluster overview** page, check the box next to the service to update and choose **Edit**\.

1. For **Task definition**, choose the task definition family and revision to use\.
**Important**  
The console validates that the selected task definition family and revision is compatible with the defined compute configuration\. If you receive a warning, verify both your task definition compatibility and the compute configuration selected\.

1. Expand the **Deployment options** section and use the following steps to change the deployment configuration for your service\.

   1. For services on AWS Fargate the platform version can be updated\.

   1. For services using a capacity provider strategy, the capacity provider strategy can be updated\.
**Note**  
A service using an Auto Scaling group capacity provider can't be updated to use a Fargate capacity provider and vice versa\.

   1. Select the **Force new deployment** option to have your service start a new deployment, which will stop all currently running tasks and launch new tasks using the updated configuration\.

   1. For **Min running tasks**, specify the lower limit on the number of tasks in the service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. For more information, see [Deployment configuration](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service_definition_parameters.html#sd-deploymentconfiguration)\.

   1. For **Max running tasks**, specify the upper limit on the number of tasks in the service that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\.

1. Expand the **Tags** section to update the tags associated with the service\.

1. Choose **Update**\.