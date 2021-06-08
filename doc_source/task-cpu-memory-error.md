# Invalid CPU or memory value specified<a name="task-cpu-memory-error"></a>

When registering a task definition using the Amazon ECS API or AWS CLI, if you specify an invalid `cpu` or `memory` value, the following error is returned\.

```
An error occurred (ClientException) when calling the RegisterTaskDefinition operation: Invalid 'cpu' setting for task. For more information, see the Troubleshooting section of the Amazon ECS Developer Guide.
```

**Note**  
When using Terraform, the following error may be returned\.  

```
Error: ClientException: No Fargate configuration exists for given values.
```

To resolve this issue, you must specify a supported value for the task CPU and memory in your task definition\. The `cpu` value can be expressed in CPU units or vCPUs in a task definition but is converted to an integer indicating the CPU units when the task definition is registered\. The `memory` value can be expressed in MiB or GB in a task definition but is converted to an integer indicating the MiB when the task definition is registered\.

For task definitions that only specify `EC2` for the `requiresCompatibilities` parameter, the supported CPU values are between `128` CPU units \(`0.125` vCPUs\) and `10240` CPU units \(`10` vCPUs\)\. The memory value must be an integer and the limit is dependent upon the amount of available memory on the underlying Amazon EC2 instance you use\.

For task definitions that specify `FARGATE` for the `requiresCompatibilities` parameter \(even if `EC2` is also specified\), you must use one of the values in the following table, which determines your range of supported values for the CPU and memory parameter\.

Supported task CPU and memory values for tasks that are hosted on Fargate are as follows\.


| CPU value | Memory value \(MiB\) | 
| --- | --- | 
| 256 \(\.25 vCPU\) | 512 \(0\.5GB\), 1024 \(1GB\), 2048 \(2GB\) | 
| 512 \(\.5 vCPU\) | 1024 \(1GB\), 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\) | 
| 1024 \(1 vCPU\) | 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\), 5120 \(5GB\), 6144 \(6GB\), 7168 \(7GB\), 8192 \(8GB\) | 
| 2048 \(2 vCPU\) | Between 4096 \(4GB\) and 16384 \(16GB\) in increments of 1024 \(1GB\) | 
| 4096 \(4 vCPU\) | Between 8192 \(8GB\) and 30720 \(30GB\) in increments of 1024 \(1GB\) | 