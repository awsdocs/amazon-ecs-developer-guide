# Deleting a task definition revision using the console<a name="delete-task-definition-v2"></a>

If you decide that you no longer need a specific task definition revision in Amazon ECS, you can deelete the task definition revision so that it no longer displays in your `ListTaskDefinitions` API calls or in the console when you want to run a task or update a service\.

When you delete a task definition revision, it immediately transitions from the `INACTIVE` to `DELETE_IN_PROGRESS`\. Existing tasks and services that reference a `DELETE_IN_PROGRESS` task definition revision continue to run without disruption\. 

You can't use a `DELETE_IN_PROGRESS` task definition revision to run new tasks or create new services\. You also can't update an existing service to reference a `DELETE_IN_PROGRESS` task definition revision\.

**To delete task definitions \(Amazon ECS console\)**

You must deregister a task definition revision before you delete it\. For more information, see [Deregistering a task definition revision using the console](deregister-task-definition-v2.md)\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, choose the region that contains your task definition\.

1. In the navigation pane, choose **Task definitions**\.

1. On the **Task definitions** page, choose the task definition family that contains one or more revisions that you want to delete\.

1. On the **task definition Name** page, select the revisions to delete, and then choose **Actions**, **Delete**\.

1. Verify the information in the **Delete** window, and then choose **Delete** to finish\.