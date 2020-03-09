# API Error Messages<a name="api_failures_messages"></a>

In some cases, an API call that you have triggered through the Amazon ECS console or the AWS CLI exits with a `failures` error message\. The following possible API `failures` error messages are explained below for each API call\. The failures occur on a particular resource, and the resource in parentheses is the resource associated with the failure\.

Many resources are region\-specific, so make sure that the console is set to the correct region for your resources\. Alternatively, make sure that your AWS CLI commands are being sent to the correct region with the `--region region` option\.
+ `DescribeClusters`  
`MISSING` \(cluster ID\)  
Your cluster was not found\. The cluster name may not have been spelled correctly or the wrong region may be specified\.
+ `DescribeInstances`  
`MISSING` \(container instance ID\)  
The container instance you are attempting to describe does not exist\. Perhaps the wrong cluster or region has been specified, or the container instance ARN or ID is misspelled\.
+ `DescribeServices`  
`MISSING` \(service ID\)  
The service you are attempting to describe does not exist\. Perhaps the wrong cluster or region has been specified, or the container instance ARN or ID is misspelled\.
+ `DescribeTasks`  
`MISSING` \(task ID\)  
The task you are trying to describe does not exist\. Perhaps the wrong cluster or region has been specified, or the task ARN or ID is misspelled\.
+ `RunTask` or `StartTask`  
`RESOURCE:*` \(container instance ID\)  
The resource or resources requested by the task are unavailable on the given container instance\. If the resource is CPU, memory, ports, or elastic network interfaces, you may need to add container instances to your cluster\. For `RESOURCE:ENI` errors, your cluster does not have any available elastic network interface attachment points, which are required for tasks that use the `awsvpc` network mode\. Amazon EC2 instances have a limit to the number of network interfaces that can be attached to them, and the primary network interface counts as one\. For more information about how many network interfaces are supported per instance type, see [IP Addresses Per Network Interface Per Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#AvailableIpPerENI) in the *Amazon EC2 User Guide for Linux Instances*\.  
`RESOURCE:GPU`  
The number of GPUs requested by the task are unavailable on the given container instance\. You may need to add GPU\-enabled container instances to your cluster\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.  
`AGENT` \(container instance ID\)  
The container instance that you attempted to launch a task onto has an agent that is currently disconnected\. To prevent extended wait times for task placement, the request was rejected\.  
`LOCATION` \(container instance ID\)  
The container instance onto which you attempted to launch a task is in a different availability zone than the availability zone of the subnet which is specified in subnets section of [awsvpcConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_AwsVpcConfiguration.html) of task with [`awsvpc` as network mode](task-networking.md)\. 
To prevent this error, you can either specify the same subnets in the awsvpcConfiguration as that of your container instance or you can specify subnets from multiple availability zones, So that the one in which your container instance is present gets selected automatically\.  
`ATTRIBUTE` \(container instance ID\)  
Your task definition contains a parameter that requires a specific container instance attribute that is not available on your container instances\. For example, if your task uses the `awsvpc` network mode, but there are no instances in your specified subnets with the `ecs.capability.task-eni` attribute\. For more information about which attributes are required for specific task definition parameters and agent configuration variables, see [Task Definition Parameters](task_definition_parameters.md) and [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
+ `StartTask`  
`MISSING` \(container instance ID\)  
The container instance you attempted to launch the task onto does not exist\. Perhaps the wrong cluster or region has been specified, or the container instance ARN or ID is misspelled\.  
`INACTIVE` \(container instance ID\)  
The container instance that you attempted to launch a task onto was previously deregistered with Amazon ECS and cannot be used\.
