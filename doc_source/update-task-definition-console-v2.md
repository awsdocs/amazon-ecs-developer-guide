# Updating a task definition using the new console<a name="update-task-definition-console-v2"></a>

To update a task definition, create a task definition revision\. If the task definition is used in a service, you must update that service to use the updated task definition\.

When you create a revision, you can modify the following container properties and environment properties\.
+ Container image URI
+ Port mappings
+ Environment variables
+ Task size
+ Container size

**To create a task definition revision**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region that contains your task definition\.

1. In the navigation pane, choose **Task definitions**\.

1. On the **Task definitions** page, choose the task, and then choose **Create new revision**\.

1. On the **Create new task definition revision** page, make changes\. For example, to change the existing container definitions \(such as the container image, memory limits, or port mappings\), select the container, and then make the changes\.

1. Verify the information, and then choose **Create**\.

1. If your task definition is used in a service, update your service with the updated task definition\. For more information, see [Updating a service](update-service.md)\.