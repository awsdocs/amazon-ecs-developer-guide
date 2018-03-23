# Cannot Pull Container Image Error<a name="task_cannot_pull_image"></a>

The following Docker errors indicate that when creating a task, the container image specified was not able to be retrieved\.

Connection timed out  
When a Fargate task is launched, its elastic network interface requires a route to the internet to pull container images\. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:  

```
CannotPullContainerError: API error (500): Get https://111122223333.dkr.ecr.us-east-1.amazonaws.com/v2/: net/http: request canceled while waiting for connection"
```
To resolve this issue, you can:  
+ For tasks in public subnets, specify **ENABLED** for **Auto\-assign public IP** when launching the task\. For more information, see [Running Tasks](ecs_run_task.md)\.
+ For tasks in private subnets, specify **DISABLED** for **Auto\-assign public IP** when launching the task, and configure a NAT Gateway in your VPC to route requests to the internet\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. For more information about creating a VPC with public and private subnets, including a NAT gateway for the private subnets, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

Image not found  
When specifying an Amazon ECR image in your container definition you must use the full ARN or URI of your ECR repository along with the image name in that repository\. If the repository or image cannot be found you will receive the following error:  

```
CannotPullContainerError: API error (404): repository 111122223333.dkr.ecr.us-east-1.amazonaws.com/<repo>/<image> not found
```
To resolve this issue, verify the repository ARN or URI and the image name\. Also ensure you have set up the proper access using the task execution IAM role\. For more information about the task execution role, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.