# Managing container swap space<a name="container-swap"></a>

Amazon ECS enables you to control the usage of swap memory space on your Linux container instances at the container level\. Using a per\-container swap configuration, each container within a task definition can have swap enabled or disabled, and for those that have it enabled, the maximum amount of swap space used can be limited\. For example, latency\-critical containers can have swap disabled, whereas containers with high transient memory demands can have swap turned on to reduce the chances of out\-of\-memory errors when the container is under load\.

The swap configuration for a container is managed by the following container definition parameters:

`maxSwap`  
The total amount of swap memory \(in MiB\) a container can use\. This parameter will be translated to the `--memory-swap` option to [docker run](https://docs.docker.com/engine/reference/run/) where the value would be the sum of the container memory plus the `maxSwap` value\.  
If a `maxSwap` value of `0` is specified, the container will not use swap\. Accepted values are `0` or any positive integer\. If the `maxSwap` parameter is omitted, the container will use the swap configuration for the container instance it is running on\. A `maxSwap` value must be set for the `swappiness` parameter to be used\.

`swappiness`  
This allows you to tune a container's memory swappiness behavior\. A `swappiness` value of `0` will cause swapping to not happen unless absolutely necessary\. A `swappiness` value of `100` will cause pages to be swapped very aggressively\. Accepted values are whole numbers between `0` and `100`\. If the `swappiness` parameter is not specified, a default value of `60` is used\. If a value is not specified for `maxSwap` then this parameter is ignored\. This parameter maps to the `--memory-swappiness` option to [docker run](https://docs.docker.com/engine/reference/run/)\.

The following is an example showing the JSON syntax:

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
+ Swap space must be enabled and allocated on the container instance for the containers to use\.
**Note**  
The Amazon ECS\-optimized AMIs do not have swap enabled by default\. You must enable swap on the instance to use this feature\. For more information, see [Instance Store Swap Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-swap-volumes.html) in the *Amazon EC2 User Guide for Linux Instances* or [How do I allocate memory to work as swap space in an Amazon EC2 instance by using a swap file?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-memory-swap-file/)\.
+ The swap space container definition parameters are only supported for task definitions using the EC2 launch type\.
+ This feature is only supported for Linux containers\.
+ If the `maxSwap` and `swappiness` container definition parameters are omitted from a task definition, each container will have a default `swappiness` value of `60` and the total swap usage will be limited to two times the memory reservation of the container\.