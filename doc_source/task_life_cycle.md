# Task Lifecycle<a name="task_life_cycle"></a>

When a task is started on a container instance, either manually or as part of a service, it can pass through several states before it finishes on its own or is stopped manually\. Some tasks are meant to run as batch jobs that naturally progress through from `PENDING` to `RUNNING` to `STOPPED`\. Other tasks, which can be part of a service, are meant to continue running indefinitely, or to be scaled up and down as needed\.

When task status changes are requested, such as stopping a task or updating the desired count of a service to scale it up or down, the Amazon ECS container agent tracks these changes as the last known status of the task and the desired status of the task\. The flow chart below shows the different paths that task status can take, based on the action that causes the status change\.

![\[Task Life Cycle\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/task-lifecycle.png)

The center path shows the natural progression of a batch job that stops on its own\. A persistent task that is not meant to finish would also be on the center path, but it would stop at the `RUNNING:RUNNING` stage\. The paths to the right show what happens at a given state if an API call reaches the agent to stop the task or a container instance\. The paths to the left show what happens if the container instance on which a task is running is removed, whether by forcefully deregistering it or by terminating the instance\.