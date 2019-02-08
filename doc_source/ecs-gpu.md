# Working with GPUs on Amazon ECS<a name="ecs-gpu"></a>

Amazon ECS supports workloads that take advantage of GPUs by enabling you to create clusters with GPU\-enabled container instances\. Amazon EC2 GPU\-based container instances using the p2 and p3 instance types provide access to NVIDIA GPUs\. For more information, see [Linux Accelerated Computing Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/accelerated-computing-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Amazon ECS provides a GPU\-optimized AMI that comes ready with pre\-configured NVIDIA kernel drivers and a Docker GPU runtime\. For more information, see [Amazon ECS GPU\-Optimized AMI](gpuami.md)\.

You can designate a number of GPUs in your task definition for task placement consideration at a container level\. Amazon ECS will schedule to available GPU\-enabled container instances and pin physical GPUs to proper containers for optimal performance\. 

The following Amazon EC2 GPU\-based instance types are supported\. For more information, see [Amazon EC2 P2 Instances](https://aws.amazon.com/ec2/instance-types/p2/) and [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/)\.


|  Instance type  |  GPUs  |  GPU Memory \(GiB\)  |  vCPUs  |  Memory \(GiB\)  | 
| --- | --- | --- | --- | --- | 
|  p2\.xlarge  |  1  |  12  |  4  |  61  | 
|  p2\.8xlarge  |  8  |  96  |  32  |  488  | 
|  p2\.16xlarge  |  16  |  192  |  64  |  732  | 
|  p3\.2xlarge  |  1  |  16  |  8  |  61  | 
|  p3\.8xlarge  |  4  |  64  |  32  |  244  | 
|  p3\.16xlarge  |  8  |  128  |  64  |  488  | 
|  p3dn\.24xlarge  |  8  |  256  |  96  |  768  | 

**Topics**
+ [Considerations for Working with GPUs](#gpu-considerations)
+ [Specifying GPUs in Your Task Definition](#ecs-gpu-specifying)

## Considerations for Working with GPUs<a name="gpu-considerations"></a>

Before you begin working with GPUs on Amazon ECS, be aware of the following considerations:
+ Your clusters can contain a mix of GPU and non\-GPU container instances\. 
+ When running a task or creating a service, you can use instance type attributes when configuring task placement constraints to ensure which of your container instances the task is launched on\. This will enable you to effectively use your resources\. For more information, see [Amazon ECS Task Placement](task-placement.md)\.

  The following example launches a task on a `p2.xlarge` container instance in your default cluster\.

  ```
  aws ecs run-task --cluster default --task-definition ecs-gpu-task-def \
  --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == p2.xlarge" --region us-east-2
  ```
+ When using the Docker CLI, the `--runtime` parameter can be used to specify the NVIDIA runtime when running a container\. For example:

  ```
  docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
  ```

## Specifying GPUs in Your Task Definition<a name="ecs-gpu-specifying"></a>

To take advantage of the GPUs on a container instance and the Docker GPU runtime, ensure you designate the number of GPUs your container requires in the task definition\. As GPU\-enabled containers are placed, the Amazon ECS container agent will pin the desired number of physical GPUs to the appropriate container\. The number of GPUs reserved for all containers in a task should not exceed the number of available GPUs on the container instance the task is launched on\. For more information, see [Creating a Task Definition](create-task-definition.md)\.

**Important**  
If your GPU requirements are not specified in the task definition, the task will use the default Docker runtime\.

The following shows the JSON format for the GPU requirements in a task definition:

```
{
  "containerDefinitions": [
     {
        ...
        "resourceRequirements" : [
            {
               "type" : "GPU", 
               "value" : "2"
            }
        ],
     },
...
}
```

The following example demonstrates the syntax for a Docker container that specifies a GPU requirement\. This container uses 2 GPUs, runs the `nvidia-smi` utility and then exits\.

```
{
  "containerDefinitions": [
    {
      "memory": 80,
      "essential": true,
      "name": "gpu",
      "image": "nvidia/cuda:9.0-base",
      "resourceRequirements": [
         {
           "type":"GPU",
           "value": "2"
         }
      ],
      "command": [
        "sh",
        "-c",
        "nvidia-smi"
      ],
      "cpu": 100
    }
  ],
  "family": "example-ecs-gpu"
}
```