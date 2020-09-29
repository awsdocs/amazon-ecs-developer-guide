# Fargate task recycling<a name="task-recycle"></a>

Amazon ECS task recycling only affects tasks using the Fargate and no notification is sent prior to the recycling event\.

A task can be recycled in the following scenarios:
+ The task is using the Fargate launch type and using platform version 1\.3\.0 or later\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.
**Note**  
Fargate tasks using platform versions prior to 1\.3\.0 are not affected\.
+ The task is part of an Amazon ECS service\. Standalone tasks are not affected by task recycling, but may still be scheduled for retirement\. For more information, see [Task retirement](task-retirement.md)\.
+ AWS determines there is cause for the task to be recycled, as described below\.

When AWS determines that a security or infrastructure update is needed for a Fargate task, it will apply the necessary patches for the task\. Most of these patches will be transparent and the task will not need to be stopped, but on occasion it is necessary for the task to be recycled\. Starting with Fargate platform version 1\.3\.0, any Fargate tasks launched as part of a service may be stopped and a new one started by the Amazon ECS service scheduler in order to provide the best possible security and availability for the task\. Task recycling begin after February 1, 2019 and will continue on a rolling basis\. The service scheduler will ensure that the desired task count for your service will be maintained\.

To prepare for this new process, we recommending testing your application behavior by simulating this scenario\. You can do this by stopping an individual task in your service to test for resiliency\.