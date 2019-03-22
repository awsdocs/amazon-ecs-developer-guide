# Amazon ECS\-optimized AMI Versions<a name="ecs-ami-versions"></a>

This topic lists the current and previous versions of the Amazon ECS\-optimized AMIs and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

**Topics**
+ [Amazon ECS\-Optimized Amazon Linux 2 AMI Versions](#al2ami-agent-versions)
+ [Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI Versions](#al2-arm64-ami-agent-versions)
+ [Amazon ECS GPU\-optimized AMI Versions](#al2-gpu-ami-agent-versions)
+ [Amazon ECS\-optimized Amazon Linux AMI Versions](#al1-ami-agent-versions)

## Amazon ECS\-Optimized Amazon Linux 2 AMI Versions<a name="al2ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20190301 | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 
| 20190215 | 1\.25\.3 | 18\.06\.1\-ce | 1\.25\.3\-1 | 
| 20190204 | 1\.25\.2 | 18\.06\.1\-ce | 1\.25\.2\-1 | 
| 20190127 | 1\.25\.1 | 18\.06\.1\-ce | 1\.25\.1\-1 | 
| 20190118 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 
| 20190107 | 1\.24\.0 | 18\.06\.1\-ce | 1\.24\.0\-1 | 
| 20181112 | 1\.22\.0 | 18\.06\.1\-ce | 1\.22\.0\-1 | 
| 20181016 | 1\.20\.3 | 18\.06\.1\-ce | 1\.21\.0\-1 | 

The current Amazon ECS\-optimized Amazon Linux 2 AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended
```

## Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI Versions<a name="al2-arm64-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20190301 | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 
| 20190215 | 1\.25\.3 | 18\.06\.1\-ce | 1\.25\.3\-1 | 
| 20190204 | 1\.25\.2 | 18\.06\.1\-ce | 1\.25\.2\-1 | 
| 20190127 | 1\.25\.1 | 18\.06\.1\-ce | 1\.25\.1\-1 | 
| 20190119 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 
| 20181120 | 1\.22\.0 | 18\.06\.1\-ce | 1\.22\.0\-1 | 

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended
```

## Amazon ECS GPU\-optimized AMI Versions<a name="al2-gpu-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


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

## Amazon ECS\-optimized Amazon Linux AMI Versions<a name="al1-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 2018\.03\.o | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 
| 2018\.03\.n | 1\.25\.3 | 18\.06\.1\-ce | 1\.25\.3\-1 | 
| 2018\.03\.m | 1\.25\.2 | 18\.06\.1\-ce | 1\.25\.2\-1 | 
| 2018\.03\.l | 1\.25\.1 | 18\.06\.1\-ce | 1\.25\.1\-1 | 
| 2018\.03\.k | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 
| 2018\.03\.j | 1\.24\.0 | 18\.06\.1\-ce | 1\.24\.0\-1 | 
| 2018\.03\.i | 1\.22\.0 | 18\.06\.1\-ce | 1\.22\.0\-1 | 
| 2018\.03\.h | 1\.21\.0 | 18\.06\.1\-ce | 1\.21\.0\-1 | 
| 2018\.03\.g | 1\.20\.3 | 18\.06\.1\-ce | 1\.20\.3\-1 | 
| 2018\.03\.f | 1\.20\.2 | 18\.06\.1\-ce | 1\.20\.2\-1 | 
| 2018\.03\.e | 1\.20\.1 | 18\.03\.1\-ce | 1\.20\.1\-1 | 
| 2018\.03\.d | 1\.20\.0 | 18\.03\.1\-ce | 1\.20\.0\-1 | 
| 2018\.03\.c | 1\.19\.1 | 18\.03\.1\-ce | 1\.19\.1\-1 | 
| 2018\.03\.b | 1\.19\.0 | 18\.03\.1\-ce | 1\.19\.0\-1 | 
| 2018\.03\.a | 1\.18\.0 | 17\.12\.1\-ce | 1\.18\.0\-1 | 
| 2017\.09\.l | 1\.17\.3 | 17\.12\.1\-ce | 1\.17\.3\-1 | 
| 2017\.09\.k | 1\.17\.2 | 17\.12\.0\-ce | 1\.17\.2\-1 | 
| 2017\.09\.j | 1\.17\.2 | 17\.12\.0\-ce | 1\.17\.2\-1 | 
| 2017\.09\.i | 1\.17\.1 | 17\.09\.1\-ce | 1\.17\.1\-1 | 
| 2017\.09\.h | 1\.17\.0 | 17\.09\.1\-ce | 1\.17\.0\-2 | 
| 2017\.09\.g | 1\.16\.2 | 17\.09\.1\-ce | 1\.16\.2\-1 | 
| 2017\.09\.f | 1\.16\.1 | 17\.06\.2\-ce | 1\.16\.1\-1 | 
| 2017\.09\.e | 1\.16\.1 | 17\.06\.2\-ce | 1\.16\.1\-1 | 
| 2017\.09\.d | 1\.16\.0 | 17\.06\.2\-ce | 1\.16\.0\-1 | 
| 2017\.09\.c | 1\.15\.2 | 17\.06\.2\-ce | 1\.15\.1\-1 | 
| 2017\.09\.b | 1\.15\.1 | 17\.06\.2\-ce | 1\.15\.1\-1 | 
| 2017\.09\.a | 1\.15\.0 | 17\.06\.2\-ce | 1\.15\.0\-4 | 
| 2017\.03\.g | 1\.14\.5 | 17\.03\.2\-ce | 1\.14\.5\-1 | 
| 2017\.03\.f | 1\.14\.4 | 17\.03\.2\-ce | 1\.14\.4\-1 | 
| 2017\.03\.e | 1\.14\.3 | 17\.03\.1\-ce | 1\.14\.3\-1 | 
| 2017\.03\.d | 1\.14\.3 | 17\.03\.1\-ce | 1\.14\.3\-1 | 
| 2017\.03\.c | 1\.14\.3 | 17\.03\.1\-ce | 1\.14\.3\-1 | 
| 2017\.03\.b | 1\.14\.3 | 17\.03\.1\-ce | 1\.14\.3\-1 | 
| 2016\.09\.g | 1\.14\.1 | 1\.12\.6 | 1\.14\.1\-1 | 
| 2016\.09\.f | 1\.14\.0 | 1\.12\.6 | 1\.14\.0\-2 | 
| 2016\.09\.e | 1\.14\.0 | 1\.12\.6 | 1\.14\.0\-1 | 
| 2016\.09\.d | 1\.13\.1 | 1\.12\.6 | 1\.13\.1\-2 | 
| 2016\.09\.c | 1\.13\.1 | 1\.11\.2 | 1\.13\.1\-1 | 
| 2016\.09\.b | 1\.13\.1 | 1\.11\.2 | 1\.13\.1\-1 | 
| 2016\.09\.a | 1\.13\.0 | 1\.11\.2 | 1\.13\.0\-1 | 
| 2016\.03\.j | 1\.13\.0 | 1\.11\.2 | 1\.13\.0\-1 | 
| 2016\.03\.i | 1\.12\.2 | 1\.11\.2 | 1\.12\.2\-1 | 
| 2016\.03\.h | 1\.12\.1 | 1\.11\.2 | 1\.12\.1\-1 | 
| 2016\.03\.g | 1\.12\.0 | 1\.11\.2 | 1\.12\.0\-1 | 
| 2016\.03\.f | 1\.11\.1 | 1\.11\.2 | 1\.11\.1\-1 | 
| 2016\.03\.e | 1\.11\.0 | 1\.11\.2 | 1\.11\.0\-1 | 
| 2016\.03\.d | 1\.10\.0 | 1\.11\.1 | 1\.10\.0\-1 | 
| 2016\.03\.c | 1\.10\.0 | 1\.11\.1 | 1\.10\.0\-1 | 
| 2016\.03\.b | 1\.9\.0 | 1\.9\.1 | 1\.9\.0\-1 | 
| 2016\.03\.a | 1\.8\.2 | 1\.9\.1 | 1\.8\.2\-1 | 
| 2015\.09\.g | 1\.8\.1 | 1\.9\.1 | 1\.8\.1\-1 | 
| 2015\.09\.f | 1\.8\.0 | 1\.9\.1 | 1\.8\.0\-1 | 
| 2015\.09\.e | 1\.7\.1 | 1\.9\.1 | 1\.7\.1\-1 | 
| 2015\.09\.d | 1\.7\.1 | 1\.9\.1 | 1\.7\.1\-1 | 
| 2015\.09\.c | 1\.7\.0 | 1\.7\.1 | 1\.7\.0\-1 | 
| 2015\.09\.b | 1\.6\.0 | 1\.7\.1 | 1\.6\.0\-1 | 
| 2015\.09\.a | 1\.5\.0 | 1\.7\.1 | 1\.5\.0\-1 | 
| 2015\.03\.g | 1\.4\.0 | 1\.7\.1 | 1\.4\.0\-2 | 
| 2015\.03\.f | 1\.4\.0 | 1\.6\.2 | 1\.4\.0\-1 | 
| 2015\.03\.e | 1\.3\.1  | 1\.6\.2 | 1\.3\.1\-1 | 
| 2015\.03\.d | 1\.2\.1  | 1\.6\.2 | 1\.2\.0\-2 | 
| 2015\.03\.c | 1\.2\.0  | 1\.6\.2 | 1\.2\.0\-1 | 
| 2015\.03\.b | 1\.1\.0 | 1\.6\.0 | 1\.0\-3 | 
| 2015\.03\.a | 1\.0\.0 | 1\.5\.0 | 1\.0\-1 | 