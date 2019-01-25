# Amazon ECS\-Optimized Amazon Linux 2 AMI Versions<a name="al2ami-agent-versions"></a>

Amazon ECS provides separate Amazon ECS\-optimized Amazon Linux 2 AMIs for x86 and arm64 architecture\. We always recommend using the latest version of the Amazon ECS\-optimized Amazon Linux 2 AMI for the architecture that meets your needs\. For more information, see [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami-get-latest.md)\.

**Amazon ECS\-optimized Amazon Linux 2 AMI**  
The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20190118 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 
| 20190107 | 1\.24\.0 | 18\.06\.1\-ce | 1\.24\.0\-1 | 
| 20181112 | 1\.22\.0 | 18\.06\.1\-ce | 1\.22\.0\-1 | 
| 20181016 | 1\.20\.3 | 18\.06\.1\-ce | 1\.21\.0\-1 | 

The current Amazon ECS\-optimized Amazon Linux 2 AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended
```

**Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI**  
The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20190119 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 
| 20181120 | 1\.22\.0 | 18\.06\.1\-ce | 1\.22\.0\-1 | 

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended
```