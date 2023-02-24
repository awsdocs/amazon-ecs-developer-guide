# Task definition states<a name="task-definition-state"></a>

The following are the possible states for a task definition:

ACTIVE  
A task definition is `ACTIVE` after it is registered with Amazon ECS\. You can use task definitions in the `ACTIVE` state to run tasks, or create services\.

INACTIVE  
A task definition transitions from the `ACTIVE` state to the `INACTIVE` state when you deregister a task definition\. You can retrieve an `INACTIVE` task definition by calling `DescribeTaskDefinition`\. You cannot run new tasks or create new services with a task definition in the `INACTIVE` state\. There is no impact on existing services or tasks\.

DELETE\_IN\_PROGRESS  
A task definition transitions from the `INACTIVE` state to the `DELETE_IN_PROGRESS` state after you submitted the task definition for deletion\. After the task definition is in the `DELETE_IN_PROGRESS` state, Amazon ECS periodically verifies that the target task definition is not being referenced by any active tasks or deployments, and then deletes the task definition permanently\. You cannot run new tasks or create new services with a task definition in the `DELETE_IN_PROGRESS` state\. A task definition can be submitted for deletion at any moment without causing impacts to existing tasks and services\.  
Task definitions that are in he `DELETE_IN_PROGRESS` state can be viewed in the console and you can retrieve the task definition by calling `DescribeTaskDefinition`\.

If you use AWS Config to manage your task definitions, AWS Config charges you for all task definition registrations\. You are only charged for deregistering the latest `ACTIVE` task definition\. There is no charge for deleting a task definition\. For more information about pricing, see [AWS Config Pricing](https://aws.amazon.com/config/pricing/)\.