# Container Instance Memory Management<a name="memory-management"></a>

When the Amazon ECS container agent registers a container instance into a cluster, the agent must determine how much memory the container instance has available to reserve for your tasks\. Because of platform memory overhead and memory occupied by the system kernel, this number is different than the installed memory amount that is advertised for Amazon EC2 instances\. For example, an `m4.large` instance has 8 GiB of installed memory\. However, this does not always translate to exactly 8192 MiB of memory available for tasks when the container instance registers\.

If you specify 8192 MiB for the task, and none of your container instances have 8192 MiB or greater of memory available to satisfy this requirement, then the task cannot be placed in your cluster\.

You should also reserve some memory for the Amazon ECS container agent and other critical system processes on your container instances, so that your task's containers do not contend for the same memory and possibly trigger a system failure\. For more information, see [Reserving System Memory](#ecs-reserved-memory)\.

The Amazon ECS container agent uses the Docker `ReadMemInfo()` function to query the total memory available to the operating system\. Both Linux and Windows provide command line utilities to determine the total memory\.

**Example \- Determine Linux total memory**  
The free command returns the total memory that is recognized by the operating system\.  

```
$ free -b
```
Example output for an `m4.large` instance running the Amazon ECS\-optimized Amazon Linux AMI\.  

```
             total       used       free     shared    buffers     cached
Mem:    8373026816  348180480 8024846336      90112   25534464  205418496
-/+ buffers/cache:  117227520 8255799296
```
This instance has 8373026816 bytes of total memory, which translates to 7985 MiB available for tasks\.

**Example \- Determine Windows total memory**  
The wmic command returns the total memory that is recognized by the operating system\.  

```
C:\> wmic ComputerSystem get TotalPhysicalMemory
```
Example output for an `m4.large` instance running the Amazon ECS\-optimized Windows AMI\.  

```
TotalPhysicalMemory
8589524992
```
This instance has 8589524992 bytes of total memory, which translates to 8191 MiB available for tasks\.

## Reserving System Memory<a name="ecs-reserved-memory"></a>

If you occupy all of the memory on a container instance with your tasks, then it is possible that your tasks will contend with critical system processes for memory and possibly trigger a system failure\. The Amazon ECS container agent provides a configuration variable called `ECS_RESERVED_MEMORY`, which you can use to remove a specified number of MiB of memory from the pool that is allocated to your tasks\. This effectively reserves that memory for critical system processes\.

For example, if you specify `ECS_RESERVED_MEMORY=256` in your container agent configuration file, then the agent registers the total memory minus 256 MiB for that instance, and 256 MiB of memory could not be allocated by ECS tasks\. For more information about agent configuration variables and how to set them, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md) and [Bootstrapping Container Instances with Amazon EC2 User Data](bootstrap_container_instance.md)\.

## Viewing Container Instance Memory<a name="viewing-memory"></a>

You can view how much memory a container instance registers with in the Amazon ECS console \(or with the [DescribeContainerInstances](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeContainerInstances.html) API operation\)\. If you are trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, you can observe the memory available for that container instance and then assign your tasks that much memory\.

**To view container instance memory**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster that hosts your container instances to view\.

1. Choose **ECS Instances**, and select a container instance from the **Container Instance** column to view\.

1. The **Resources** section shows the registered and available memory for the container instance\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/instance-memory.png)

   The **Registered** memory value is what the container instance registered with Amazon ECS when it was first launched, and the **Available** memory value is what has not already been allocated to tasks\.