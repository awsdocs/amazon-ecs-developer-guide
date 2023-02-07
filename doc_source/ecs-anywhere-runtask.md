# Running workloads on external instances<a name="ecs-anywhere-runtask"></a>

After an external instance is registered to your cluster, you can run your containerized workloads\. The first step is to register a task definition\. Amazon ECS provides the `requiresCompatibilities` parameter to validate the task definition is compatible with your external instances\. When you deploy your workload, use the `EXTERNAL` launch type when creating your service or running your standalone task\.

## Creating a task definition that is compatible with your external instances<a name="ecs-anywhere-taskdef"></a>

When registering an Amazon ECS task definition, use the `requiresCompatibilities` parameter and specify `EXTERNAL` which validates that the task definition is compatible to use when running Amazon ECS workloads on your external instances\. For more information, see [Creating a task definition using the console](create-task-definition.md)\.

**Important**  
If your tasks require a task execution IAM role, make sure that it's specified in the task definition\. For more information, see [Conditional IAM permissions](ecs-anywhere-iam.md#ecs-anywhere-iam-conditional)\.

The following is an example task definition\.

------
#### [ Linux ]

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

------
#### [ Windows ]

```
{
	"requiresCompatibilities": [
		"EXTERNAL"
	],
	"containerDefinitions": [{
		"name": "windows-container",
		"image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
		"memory": 256,
		"cpu": 512,
		"essential": true,
		"portMappings": [{
			"containerPort": 80,
			"hostPort": 8080,
			"protocol": "tcp"
		}]
	}],
	"networkMode": "bridge",
	"family": "windows-container"
}
```

------

## Running a standalone task or creating a service<a name="ecs-anywhere-running"></a>

After you register your external instances to your cluster, grant the relevant IAM permissions, and register a valid task definition, you can start to run your workloads on Amazon ECS\. When running your standalone tasks or creating a service, specify the `EXTERNAL` launch type, and the Amazon ECS scheduler places the tasks on your external instances\.

For instructions on how to create services, see [Creating an Amazon ECS service in the classic console](create-service.md)\.

For more information about running standalone tasks, see [Run a standalone task in the classic Amazon ECS console](ecs_run_task.md)\.

## Verifying your running tasks on an external Windows instance<a name="ecs-anywhere-windows-verifying"></a>

After you launch a task, you can verify the status by checking the Amazon ECS agent log \(`ecs-agent.log`\) which is located in the `\ProgramData\Amazon\ECS\log` directory\. 