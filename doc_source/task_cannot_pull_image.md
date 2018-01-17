# Cannot Pull Container Image Error<a name="task_cannot_pull_image"></a>

When a Fargate task is launched, it requires a public IP address to be assigned to the task elastic network interface, and a route to the internet, or a NAT gateway that can route requests to the internet, to pull container images\. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:

```
CannotPullContainerError: API error (500): Get https://111122223333.dkr.ecr.us-east-1.amazonaws.com/v2/: net/http: request canceled while waiting for connection"
```

To resolve this issue, you must specify **ENABLED** for **Auto\-assign public IP** when launching the task\. For more information, see [Running Tasks](ecs_run_task.md)\.