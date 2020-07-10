# Task lifecycle<a name="task-lifecycle"></a>

When a task is started, either manually or as part of a service, it can pass through several states before it finishes on its own or is stopped manually\. Some tasks are meant to run as batch jobs that naturally progress through from `PENDING` to `RUNNING` to `STOPPED`\. Other tasks, which can be part of a service, are meant to continue running indefinitely, or to be scaled up and down as needed\.

When task status changes are requested, such as stopping a task or updating the desired count of a service to scale it up or down, the Amazon ECS container agent tracks these changes as the last known status \(`lastStatus`\) of the task and the desired status \(`desiredStatus`\) of the task\. Both the last known status and desired status of a task can be seen either in the console or by describing the task with the API or AWS CLI\.

The flow chart below shows the task lifecycle flow\.

![\[Task Lifecycle\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/task-lifecycle.png)

## Lifecycle states<a name="lifecycle-states"></a>

The following are descriptions of each of the task lifecycle states\.

PROVISIONING  
Amazon ECS has to perform additional steps before the task is launched\. For example, for tasks that use the `awsvpc` network mode, the elastic network interface needs to be provisioned\.

PENDING  
This is a transition state where Amazon ECS is waiting on the container agent to take further action\.

ACTIVATING  
Amazon ECS has to perform additional steps after the task is launched but before the task can transition to the `RUNNING` state\. For example, for tasks that have service discovery configured, the service discovery resources must be created\. For tasks that are part of a service that is configured to use multiple Elastic Load Balancing target groups, the target group registration occurs during this state\.

RUNNING  
The task is successfully running\.

DEACTIVATING  
Amazon ECS has to perform additional steps before the task is stopped\. For example, for tasks that are part of a service that is configured to use multiple Elastic Load Balancing target groups, the target group deregistration occurs during this state\.

STOPPING  
This is a transition state where Amazon ECS is waiting on the container agent to take further action\.

DEPROVISIONING  
Amazon ECS has to perform additional steps after the task has stopped but before the task transitions to the `STOPPED` state\. For example, for tasks that use the `awsvpc` network mode, the elastic network interface needs to be detached and deleted\.

STOPPED  
The task has been successfully stopped\.