# Task networking with the `bridge` network mode<a name="task-networking-bridge"></a>

Amazon ECS tasks using the `bridge` network mode utilize Docker's built\-in virtual network which runs inside each container\. The bridge is an internal network namespace that allows each container connected to the same bridge network to communicate with each other\. It also provides an isolation boundary from containers that aren't connected to the same bridge network\.

With the `bridge` network mode, you use static or dynamic port mappings to map ports in the container with ports on the Amazon EC2 host\. For more information, see [Choosing a network mode](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/networking-networkmode.html) in the *Amazon ECS Best Practices Guide*\.