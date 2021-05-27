# Running workloads on external instances<a name="ecs-anywhere-runtask"></a>

With external instances registered to your cluster, you are ready to run your containerized workloads on them\. The first step is to register a task definition\. Amazon ECS provides the `requiresCompatibilities` parameter to validate the task definition is compatible with your external instances\. Once you are ready to deploy your workload, simply use the `EXTERNAL` launch type when creating your service or running your standalone task\.

## Creating a task definition that is compatible with your external instances<a name="ecs-anywhere-taskdef"></a>

When registering an Amazon ECS task definition, use the `requiresCompatibilities` parameter and specify `EXTERNAL` which validates that the task definition is compatible to use when running Amazon ECS workloads on your external instances\. For more information, see [Creating a task definition](create-task-definition.md)\.

**Important**  
If your tasks require a task execution IAM role, ensure that it is specified in the task definition\. For more information, see [Conditional IAM permissions](ecs-anywhere-iam.md#ecs-anywhere-iam-conditional)\.

The following is an example task definition\.

```
{
	"requiresCompatibilities": [
		"EXTERNAL"
	],
	"containerDefinitions": [{
		"name": "nginx",
		"image": "public.ecr.aws/nginx/nginx:latest",
		"memory": 256,
		"cpu": 256,
		"essential": true,
		"portMappings": [{
			"containerPort": 80,
			"hostPort": 8080,
			"protocol": "tcp"
		}]
	}],
	"networkMode": "bridge",
	"family": "nginx"
}
```

## Running a standalone task or creating a service<a name="ecs-anywhere-running"></a>

Once you have your external instances registered to your cluster, the IAM permissions in place, and a valid task definition, you are ready to start running your workloads on Amazon ECS\. When running your standalone tasks or creating a service, simply specify the `EXTERNAL` launch type and the Amazon ECS scheduler will place the tasks on your external instances\.

For more information on creating services, see [Creating an Amazon ECS service](create-service.md)\.

For more information on running standalone tasks, see [Run a standalone task](ecs_run_task.md)\.