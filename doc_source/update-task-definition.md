# Updating a task definition using the classic console<a name="update-task-definition"></a>


|  | 
| --- |
| The new experience is now the default in the Amazon ECS console\. For more information, see [Updating a task definition using the console](update-task-definition-console-v2.md)\. | 

A *task definition revision* is a copy of the current task definition with the new parameter values replacing the existing ones\. All parameters that you do not modify are in the new revision\.

To update a task definition, create a task definition revision\. If the task definition is used in a service, you must update that service to use the updated task definition\.

**To create a task definition revision**

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region that contains your task definition\.

1. In the navigation pane, choose **task definitions**\.

1. On the **task definitions** page, select the box to the left of the task definition to revise and choose **Create new revision**\.

1. On the **Create new revision of task definition** page, make changes\. For example, to change the existing container definitions \(such as the container image, memory limits, or port mappings\), select the container, make the changes, and then choose **Update**\.

1. Verify the information and choose **Create**\.

1. If your task definition is used in a service, update your service with the updated task definition\. For more information, see [Updating a service using the console](update-service-console-v2.md)\.