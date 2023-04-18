# Managing container swap space<a name="container-swap"></a>

With Amazon ECS, you can control the usage of swap memory space on your Linux\-based Amazon EC2 instances at the container level\. Using a per\-container swap configuration, each container within a task definition can have swap enabled or disabled\. For those that have it enabled, the maximum amount of swap space that's used can be limited\. For example, latency\-critical containers can have swap disabled\. In contrast, containers with high transient memory demands can have swap turned on to reduce the chances of out\-of\-memory errors when the container is under load\.

The swap configuration for a container is managed by the following container definition parameters\.

`maxSwap`  
The total amount of swap memory \(in MiB\) a container can use\. This parameter is translated to the `--memory-swap` option to [docker run](https://docs.docker.com/engine/reference/run/) where the value is the sum of the container memory plus the `maxSwap` value\.  
If a `maxSwap` value of `0` is specified, the container doesn't use swap\. Accepted values are `0` or any positive integer\. If the `maxSwap` parameter is omitted, the container uses the swap configuration for the container instance that it's running on\. A `maxSwap` value must be set for the `swappiness` parameter to be used\.

`swappiness`  
You can use this to tune a container's memory swappiness behavior\. A `swappiness` value of `0` causes swapping to not occur unless required\. A `swappiness` value of `100` causes pages to be swapped aggressively\. Accepted values are whole numbers between `0` and `100`\. If the `swappiness` parameter isn't specified, a default value of `60` is used\. If a value isn't specified for `maxSwap`, this parameter is ignored\. This parameter maps to the `--memory-swappiness` option to [docker run](https://docs.docker.com/engine/reference/run/)\.

In the following example, the JSON syntax is provided\.

```
"containerDefinitions": [{
        ...
        "linuxParameters": {
            "maxSwap": integer,
            "swappiness": integer
        },
        ...
}]
```

## Container swap considerations<a name="container-swap-considerations"></a>

Consider the following when you use a per\-container swap configuration\.
+ Swap space must be enabled and allocated on the Amazon EC2 instance hosting your tasks for the containers to use\. By default, the Amazon ECS optimized AMIs do not have swap enabled\. You must enable swap on the instance to use this feature\. For more information, see [Instance Store Swap Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-swap-volumes.html) in the *Amazon EC2 User Guide for Linux Instances* or [How do I allocate memory to work as swap space in an Amazon EC2 instance by using a swap file?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-memory-swap-file/)\.
+ The swap space container definition parameters are only supported for task definitions that specify the EC2 launch type\. These parameters are not supported for task definitions intended only for Amazon ECS on Fargate use\.
+ This feature is only supported for Linux containers\. Windows containers are not supported currently\.
+ If the `maxSwap` and `swappiness` container definition parameters are omitted from a task definition, each container has a default `swappiness` value of `60`\. Moreover, the total swap usage is limited to two times the memory reservation of the container\.
+ If you're using tasks on Amazon Linux 2023 the `swappiness` parameter isn't supported\.