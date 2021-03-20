# Updating a service using the old console<a name="update-service-console-v1"></a>

**Important**  
Amazon ECS has provided a new console experience for updating a service\. For more information, see [Updating a service using the new console](update-service-console-v2.md)\.

**To update a running service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the Region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster in which your service resides\.

1. On the **Cluster: *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Update**\.

1. On the **Configure service** page, your service information is pre\-populated\. Change the task definition, capacity provider strategy, platform version, deployment configuration, or number of desired tasks \(or any combination of these\)\. To have your service start a new deployment, which will stop and relaunch all tasks using the new configuration, select **Force new deployment**\. Choose **Next step** when finished changing the service configuration\.
**Note**  
A service using an Auto Scaling group capacity provider can't be updated to use a Fargate capacity provider and vice versa\.

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