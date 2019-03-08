# Amazon ECS GPU\-Optimized AMI Versions<a name="gpuami-agent-versions"></a>

Amazon ECS provides a Amazon ECS GPU\-optimized AMI for use with GPU workloads\. We always recommend using the latest version of the Amazon ECS GPU\-optimized AMI\. For more information, see [How to Launch the Latest Amazon ECS GPU\-Optimized AMI](gpuami-get-latest.md)\.

**Amazon ECS GPU\-optimized AMI**  
The following table lists the current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS GPU\-optimized AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20190301 | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 
| 20190215 | 1\.25\.3 | 18\.06\.1\-ce | 1\.25\.3\-1 | 
| 20190204 | 1\.25\.2 | 18\.06\.1\-ce | 1\.25\.2\-1 | 
| 20190127 | 1\.25\.1 | 18\.06\.1\-ce | 1\.25\.1\-1 | 
| 20190118 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 

You can retrieve the current Amazon ECS GPU\-optimized AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended
```