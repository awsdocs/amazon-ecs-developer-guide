# Updating a task definition using the new console<a name="update-task-definition-console-v2"></a>

A *task definition revision* is a copy of the current task definition with the new parameter values replacing the existing ones\. All parameters that you do not modify are in the new revision\.

To update a task definition, create a task definition revision\. If the task definition is used in a service, you must update that service to use the updated task definition\.

When you create a revision, you can modify the following container properties and environment properties\.
+ Container image URI
+ Port mappings
+ Environment variables
+ Task size
+ Container size
+ Task role
+ Task execution role

## JSON validation<a name="json-validate-for-update"></a>

The Amazon ECS console JON editor validates the following in the JSON file:
+ The file is a valid JSON file
+ The file does not contain any extraneous keys
+ The file contains the `familyName` parameter
+ There is at least one entry under `containerDefinitions`

------
#### [ New Amazon ECS console ]

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, choose the Region that contains your task definition\.

1. In the navigation pane, choose **Task definitions**\.

1. On the **Task definitions** page, choose the task, and then choose **Create new revision**, **Create new revision**\.

1. On the **Create new task definition revision** page, make changes\. For example, to change the existing container definitions \(such as the container image, memory limits, or port mappings\), select the container, and then make the changes\.

1. Verify the information, and then choose **Create**\.

1. If your task definition is used in a service, update your service with the updated task definition\. For more information, see [Updating a service using the new console](update-service-console-v2.md)\.

------
#### [ New Amazon ECS console JSON editor ]

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**\.

1. Choose **Create new revision**, **Create new revision with JSON**\.

1. In the JSON editor box, edit your JSON file,

   The JSON must pass the validation checks specified in [JSON validation](#json-validate-for-update)\.

1. Choose **Create**\.

------