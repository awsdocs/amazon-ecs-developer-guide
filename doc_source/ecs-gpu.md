# Working with GPUs on Amazon ECS<a name="ecs-gpu"></a>

Amazon ECS supports workloads that take advantage of GPUs by enabling you to create clusters with GPU\-enabled container instances\. Amazon EC2 GPU\-based container instances using the p2, p3, g3, and g4 instance types provide access to NVIDIA GPUs\. For more information, see [Linux Accelerated Computing Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/accelerated-computing-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Amazon ECS provides a GPU\-optimized AMI that comes ready with pre\-configured NVIDIA kernel drivers and a Docker GPU runtime\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

You can designate a number of GPUs in your task definition for task placement consideration at a container level\. Amazon ECS will schedule to available GPU\-enabled container instances and pin physical GPUs to proper containers for optimal performance\. 

The following Amazon EC2 GPU\-based instance types are supported\. For more information, see [Amazon EC2 P2 Instances](https://aws.amazon.com/ec2/instance-types/p2/), [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/), [Amazon EC2 G3 Instances](https://aws.amazon.com/ec2/instance-types/g3/), and [Amazon EC2 G4 Instances](https://aws.amazon.com/ec2/instance-types/g4/)\.

**Important**  
The g4 instance type family is supported on version `20190913` and later of the Amazon ECS GPU\-optimized AMI\. For more information, see [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux)\. It is currently not supported in the Create Cluster workflow in the Amazon ECS console\. To use these instance types, you must either use the Amazon EC2 console, AWS CLI, or API and manually register the instances to your cluster\.


|  Instance type  |  GPUs  |  GPU memory \(GiB\)  |  vCPUs  |  Memory \(GiB\)  | 
| --- | --- | --- | --- | --- | 
|  p2\.xlarge  |  1  |  12  |  4  |  61  | 
|  p2\.8xlarge  |  8  |  96  |  32  |  488  | 
|  p2\.16xlarge  |  16  |  192  |  64  |  732  | 
|  p3\.2xlarge  |  1  |  16  |  8  |  61  | 
|  p3\.8xlarge  |  4  |  64  |  32  |  244  | 
|  p3\.16xlarge  |  8  |  128  |  64  |  488  | 
|  p3dn\.24xlarge  |  8  |  256  |  96  |  768  | 
|  g3s\.xlarge  | 1 | 8 | 4 | 30\.5 | 
|  g3\.4xlarge  | 1 | 8 | 16 | 122 | 
|  g3\.8xlarge  | 2 | 16 | 32 | 244 | 
|  g3\.16xlarge  | 4 | 32 | 64 | 488 | 
|  g4dn\.xlarge  | 1 | 16 | 4 | 16 | 
|  g4dn\.2xlarge  | 1 | 16 | 8 | 32 | 
|  g4dn\.4xlarge  | 1 | 16 | 16 | 64 | 
|  g4dn\.8xlarge  | 1 | 16 | 32 | 128 | 
|  g4dn\.12xlarge  | 4 | 64 | 48 | 192 | 
|  g4dn\.16xlarge  | 1 | 16 | 64 | 256 | 

**Topics**
+ [Considerations](#gpu-considerations)
+ [Specifying GPUs in your task definition](#ecs-gpu-specifying)

## Considerations<a name="gpu-considerations"></a>

Before you begin working with GPUs on Amazon ECS, be aware of the following considerations:
+ Your clusters can contain a mix of GPU and non\-GPU container instances\. 
+ When running a task or creating a service, you can use instance type attributes when configuring task placement constraints to ensure which of your container instances the task is launched on\. This will enable you to effectively use your resources\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  The following example launches a task on a `p2.xlarge` container instance in your default cluster\.

  ```
  aws ecs run-task --cluster default --task-definition ecs-gpu-task-def \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == p2.xlarge" --region us-east-2
  ```
+ For each container that has a GPU resource requirement specified in the container definition, Amazon ECS sets the container runtime to be the NVIDIA container runtime\.
+ The NVIDIA container runtime requires some environment variables to be set in the container in order to work\. For a list of these environment variables, see [nvidia\-container\-runtime](https://github.com/NVIDIA/nvidia-container-runtime)\. Amazon ECS sets the `NVIDIA_VISIBLE_DEVICES` environment variable value to be a list of the GPU device IDs that Amazon ECS assigns to the container\. For the other required environment variables, Amazon ECS does not set them and you should ensure that your container image sets them or they should be set in the container definition\.

## Specifying GPUs in your task definition<a name="ecs-gpu-specifying"></a>

To take advantage of the GPUs on a container instance and the Docker GPU runtime, ensure you designate the number of GPUs your container requires in the task definition\. As GPU\-enabled containers are placed, the Amazon ECS container agent will pin the desired number of physical GPUs to the appropriate container\. The number of GPUs reserved for all containers in a task should not exceed the number of available GPUs on the container instance the task is launched on\. For more information, see [Creating a task definition](create-task-definition.md)\.

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