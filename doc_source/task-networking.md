# Amazon ECS task networking<a name="task-networking"></a>

**Important**  
If you're using Amazon ECS tasks hosted on AWS Fargate, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate* for networking information that's relevant to your instances\.

The networking behavior of Amazon ECS tasks that are hosted on Amazon EC2 instances is dependent on the *network mode* that's defined in the task definition\. We recommend that you use the `awsvpc` network mode unless you have a specific need to use a different network mode\.

The following are the available network modes\.


| Network mode | Linux containers on EC2 | Windows containers on EC2 | Description | 
| --- | --- | --- | --- | 
|  `awsvpc`  |  Yes  |  Yes  |  The task is allocated its own elastic network interface \(ENI\) and a primary private IPv4 address\. This gives the task the same networking properties as Amazon EC2 instances\.  | 
|  `bridge`  |  Yes  |  No  |  The task uses Docker's built\-in virtual network on Linux, which runs inside each Amazon EC2 instance that hosts the task\. The built\-in virtual network on Linux uses the `bridge` Docker network driver\. This is the default network mode on Linux if a network mode isn't specified in the task definition\.  | 
|  `host`  |  Yes  |  No  |  The task uses the host's network which bypasses Docker's built\-in virtual network by mapping container ports directly to the ENI of the Amazon EC2 instance that hosts the task\. Dynamic port mappings can’t be used in this network mode\. A container in a task definition that uses this mode must specify a specific `hostPort` number\. A port number on a host can’t be used by multiple tasks\. As a result, you can’t run multiple tasks of the same task definition on a single Amazon EC2 instance\.  | 
|  `none`  |  Yes  |  No  |  The task has no external network connectivity\.  | 
|  `default`  |  No  |  Yes  |  The task uses Docker's built\-in virtual network on Windows, which runs inside each Amazon EC2 instance that hosts the task\. The built\-in virtual network on Windows uses the `nat` Docker network driver\. This is the default network mode on Windows if a network mode isn't specified in the task definition\.  | 

For more information about Docker networking on Linux, see [Networking overview](https://docs.docker.com/network/) in the *Docker Documentation*\.

For more information about Docker networking on Windows, see [Windows container networking](https://learn.microsoft.com/en-us/virtualization/windowscontainers/container-networking/architecture) in the Microsoft *Containers on Windows Documentation*\.

**Topics**
+ [Task networking with the `awsvpc` network mode](task-networking-awsvpc.md)
+ [Task networking with the `bridge` network mode](task-networking-bridge.md)
+ [Task networking with the `host` network mode](task-networking-host.md)