# Updating a service using the new console<a name="update-service-console-v2"></a>

You can update an Amazon ECS service using the new Amazon ECS console\. When updating a service using the AWS Management Console, the current service configuration is pre\-populated\. You are able to update the task definition, desired task count, capacity provider strategy, platform version, and deployment configuration; or any combination of these\.

**Note**  
Currently, only services using the **Rolling update** \(`ECS`\) deployment type should be updated using the new console\. To update a service using any other deployment type, switch to the old console\.

**To create a service using the new console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster to create the service in\.

1. On the **Cluster overview** page, check the box next to the service to update and choose **Edit**\.

1. For **Task definition**, choose the task definition family and revision to use\.
**Important**  
The console validates that the selected task definition family and revision is compatible with the defined compute configuration\. If you receive a warning, verify both your task definition compatibility and the compute configuration selected\.

1. Expand the **Deployment options** section and use the following steps to change the deployment configuration for your service\.

   1. For services on AWS Fargate the platform version can be updated\.

   1. For services using a capacity provider strategy, the capacity provider strategy can be updated\.

   1. Select the **Force new deployment** option to have your service start a new deployment, which will stop all currently running tasks and launch new tasks using the updated configuration\.

   1. For **Min running tasks**, specify the lower limit on the number of tasks in the service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\.

   1. For **Max running tasks**, specify the upper limit on the number of tasks in the service that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\.

1. Expand the **Tags** section to update the tags associated with the service\.

1. Choose **Update**\.