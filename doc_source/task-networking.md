# Amazon ECS task networking<a name="task-networking"></a>

**Important**  
If you're using Amazon ECS tasks hosted on AWS Fargate, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate* for networking information that's relevant to your instances\.

The networking behavior of Amazon ECS tasks that are hosted on Amazon EC2 instances is dependent on the *network mode* that's defined in the task definition\. The following are the available network modes\. We recommend that you use the `awsvpc` network mode unless you have a specific need to use a different network mode\.
+ `awsvpc` — The task is allocated its own elastic network interface \(ENI\) and a primary private IPv4 address\. This gives the task the same networking properties as Amazon EC2 instances\.
+ `bridge` — The task uses Docker's built\-in virtual network, which runs inside each Amazon EC2 instance that hosts the task\.
+ `host` — The task bypasses Docker's built\-in virtual network and maps container ports directly to the ENI of the Amazon EC2 instance that hosts the task\. As a result, you can't run multiple instantiations of the same task on a single Amazon EC2 instance when port mappings are used\.
+ `none` — The task has no external network connectivity\.

For more information about Docker networking, see [Networking overview](https://docs.docker.com/network/)\.

**Topics**
+ [Task networking with the `awsvpc` network mode](task-networking-awsvpc.md)
+ [Task networking with the `bridge` network mode](task-networking-bridge.md)
+ [Task networking with the `host` network mode](task-networking-host.md)