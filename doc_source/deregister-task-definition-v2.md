# Deregistering a task definition revision using the new console<a name="deregister-task-definition-v2"></a>

If you decide that you no longer need a specific task definition revision in Amazon ECS, you can deregister the task definition revision so that it no longer displays in your `ListTaskDefinition` API calls or in the console when you want to run a task or update a service\.

When you deregister a task definition revision, it is immediately marked as `INACTIVE`\. Existing tasks and services that reference an `INACTIVE` task definition revision continue to run without disruption\. Existing services that reference an `INACTIVE` task definition revision can still scale up or down by modifying the service's desired count\.

You can't use an `INACTIVE` task definition revision to run new tasks or create new services\. You also can't update an existing service to reference an `INACTIVE` task definition revision \(even though there may be up to a 10\-minute window following deregistration where these restrictions have not yet taken effect\)\.

**Note**  
At this time, `INACTIVE` task definition revisions remain discoverable in your account indefinitely\. However, this behavior is subject to change in the future\. Therefore, you should not rely on `INACTIVE` task definition revisions persisting beyond the lifecycle of any associated tasks and services\.

**To deregister a new task definition \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, choose the region that contains your task definition\.

1. In the navigation pane, choose **Task definitions**\.

1. On the **task definitions** page, choose the task definition family that contains one or more revisions that you want to deregister\.

1. On the **task definition Name** page, choose the revision you want to deregister, such as "example\-task:1"\.

1. In the upper\-right of the task definition revision detail page, choose **Deregister**\.

1. Verify the information in the **Deregister** window, and then choose **Deregister** to finish\.