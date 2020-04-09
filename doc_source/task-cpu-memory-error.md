# Invalid CPU or memory value specified<a name="task-cpu-memory-error"></a>

When registering a task, if you specify an invalid `cpu` or `memory` value, you receive the following error:

```
An error occurred (ClientException) when calling the RegisterTaskDefinition operation: Invalid 'cpu' setting for task. For more information, see the Troubleshooting section of the Amazon ECS Developer Guide.
```

To resolve this issue, you must specify a supported value for the task CPU and memory in your task definition\.

The `cpu` value can be expressed in CPU units or vCPUs in a task definition but is converted to an integer indicating the CPU units when the task definition is registered\. If you are using the EC2 launch type, the supported values are between `128` CPU units \(`0.125` vCPUs\) and `10240` CPU units \(`10` vCPUs\)\. If you are using the Fargate launch type, you must use one of the values in the following table, which determines your range of supported values for the `memory` parameter\.

The `memory` value can be expressed in MiB or GB in a task definition but is converted to an integer indicating the MiB when the task definition is registered\. If you are using the EC2 launch type, you must specify an integer\. If you are using the Fargate launch type, you must use one of the values in the following table, which determines your range of supported values for the `cpu` parameter\.

Supported task CPU and memory values for Fargate tasks are as follows\.


| CPU value | Memory value \(MiB\) | 
| --- | --- | 
| 256 \(\.25 vCPU\) | 512 \(0\.5GB\), 1024 \(1GB\), 2048 \(2GB\) | 
| 512 \(\.5 vCPU\) | 1024 \(1GB\), 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\) | 
| 1024 \(1 vCPU\) | 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\), 5120 \(5GB\), 6144 \(6GB\), 7168 \(7GB\), 8192 \(8GB\) | 
| 2048 \(2 vCPU\) | Between 4096 \(4GB\) and 16384 \(16GB\) in increments of 1024 \(1GB\) | 
| 4096 \(4 vCPU\) | Between 8192 \(8GB\) and 30720 \(30GB\) in increments of 1024 \(1GB\) | 