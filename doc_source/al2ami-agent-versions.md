# Amazon ECS\-Optimized Amazon Linux 2 AMI Versions<a name="al2ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.

The current Amazon ECS\-optimized Amazon Linux 2 AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended
```

We always recommend using the latest version of the Amazon ECS\-optimized Amazon Linux 2 AMI\. For more information, see [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami-get-latest.md)\.


| Amazon ECS\-optimized Amazon Linux 2 AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20181016 | 1\.20\.3 | 18\.06\.1\-ce | 1\.21\.0\-1 | 