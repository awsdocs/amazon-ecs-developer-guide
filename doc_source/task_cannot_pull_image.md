# Cannot Pull Container Image Error<a name="task_cannot_pull_image"></a>

The following Docker errors indicate that when creating a task, the container image specified was not able to be retrieved\.

Connection timed out  
When a Fargate task is launched, it requires a public IP address to be assigned to the task elastic network interface, and a route to the internet, or a NAT gateway that can route requests to the internet, to pull container images\. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:  

```
CannotPullContainerError: API error (500): Get https://111122223333.dkr.ecr.us-east-1.amazonaws.com/v2/: net/http: request canceled while waiting for connection"
```
To resolve this issue, you must specify **ENABLED** for **Auto\-assign public IP** when launching the task\. For more information, see [Running Tasks](ecs_run_task.md)\.

Image not found  
When specifying an Amazon ECR image in your container definition you must use the full ARN or URI of your ECR repository along with the image name in that repository\. If the repository or image cannot be found you will receive the following error:  

```
CannotPullContainerError: API error (404): repository 111122223333.dkr.ecr.us-east-1.amazonaws.com/<repo>/<image> not found
```
To resolve this issue, verify the repository ARN or URI and the image name\. Also ensure you have set up the proper access using the task execution IAM role\. For more information about the task execution role, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.