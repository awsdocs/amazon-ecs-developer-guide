# Working with 64\-bit ARM workloads on Amazon ECS<a name="ecs-arm64"></a>

Amazon ECS supports using 64\-bit ARM applications\. You can run your applications on the platform that's powered by [AWS Graviton2](http://aws.amazon.com/ec2/graviton/) processors,\. It's suitable for a wide variety of workloads\. This includes workloads such as application servers, micro\-services, high\-performance computing, CPU\-based machine learning inference, video encoding, electronic design automation, gaming, open\-source databases, and in\-memory caches\.

## Considerations<a name="ecs-arm64-considerations"></a>

Before you begin deploying task definitions that use the 64\-bit ARM architecture, consider the following:
+ The applications can use the Fargate or EC2 launch types\.
+ The applications can only use the Linux operating system\.
+ For the Fargate type, the applications must use Fargate platform version `1.4.0` or later \.
+ The applications can use Fluent Bit or CloudWatch for monitoring\.
+ For the Fargate launch type, the following AWS Regions do not support 64\-bit ARM workloads:
  + US East \(N\. Virginia\) , the `use1-az3` Availability Zone
  + Asia Pacific \(Hyderabad\)
  + China \(Beijing\)
  + China \(Ningxia\)
  +  Europe \(Zurich\)
  +  Africa \(Cape Town\)
  + Middle East \(UAE\)
  + Middle East \(Bahrain\)
  + AWS GovCloud \(US\-East\)
  + AWS GovCloud \(US\-West\)
  + In Asia Pacific \(Osaka\), specifically the `apne3-az2` and `apne3-az3` Availability Zones only
+  For the Amazon EC2 launch type, see the following to verify that the Region that you're in supports the instance type you want to use:
  + [Amazon EC2 M6g Instances](https://aws.amazon.com/ec2/instance-types/m6)
  +  [Amazon EC2 T4g Instances](http://aws.amazon.com/ec2/instance-types/t4/)
  +  [Amazon EC2 C6g Instances](http://aws.amazon.com/ec2/instance-types/c6g/)
  +  [Amazon EC2 R6gd Instances](http://aws.amazon.com/ec2/instance-types/r6/)
  +  [Amazon EC2 X2gd Instances](http://aws.amazon.com/ec2/instance-types/x2/)

  You can also use the Amazon EC2 `describe-instance-type-offerings` command with a filter to view the instance offering for your Region\. 

  ```
  aws ec2 describe-instance-type-offerings --filters Name=instance-type,Values=instance-type --region region
  ```

  The following example checks for the M6 instance type availability in the US East \(N\. Virginia\) \(us\-east\-1\) Region\.

  ```
  aws ec2 describe-instance-type-offerings --filters Name=instance-type,Values=M6 --region us-east-1
  ```

  For more information, see [describe\-instance\-type\-offerings ](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instance-type-offerings.html)in the *Amazon EC2 Command Line Reference*\.

## Specifying the ARM architecture in your task definition<a name="ecs-arm-specifying"></a>

To use the ARM architecture, specify `ARM64` for the `cpuArchitecture` task definition parameter\. 

In the following example, the ARM architecture is specified in a task definition\. It's in JSON format\.

```
{
    "runtimePlatform": {
        "operatingSystemFamily": "LINUX",
        "cpuArchitecture": "ARM64"
    },
...
}
```

In the following example, a task definition for the ARM architecture displays "hello world\."

```
{
 "family": "arm64-testapp",
 "networkMode": "awsvpc",
 "containerDefinitions": [
    {
        "name": "arm-container",
        "image": "arm64v8/busybox",
        "cpu": 100,
        "memory": 100,
        "essential": true,
        "command": [ "echo hello world" ],
        "entryPoint": [ "sh", "-c" ]
    }
 ],
 "requiresCompatibilities": [ "FARGATE" ],
 "cpu": "256",
 "memory": "512",
 "runtimePlatform": {
        "operatingSystemFamily": "LINUX",
        "cpuArchitecture": "ARM64"
  },
 "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole"
}
```

## Interfaces for configuring ARM<a name="Interfaces"></a>

You can configure the ARM CPU architecture for Amazon ECS task definitions using one of the following interfaces:
+ New Amazon ECS console 
+ AWS Command Line Interface \(AWS CLI\) 
+ AWS SDKs 
+ AWS Copilot 