# Amazon ECS\-optimized AMI Versions<a name="ecs-ami-versions"></a>

This topic lists the current and previous versions of the Amazon ECS\-optimized AMIs and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

## Amazon ECS\-optimized Amazon Linux 2 AMI Versions<a name="al2ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20200603 | 1\.40\.0 | 19\.03\.6\-ce | 1\.40\.0\-1 | 
| 20200430 | 1\.39\.0 | 19\.03\.6\-ce | 1\.39\.0\-1 | 
| 20200402 | 1\.39\.0 | 18\.09\.9\-ce | 1\.39\.0\-1 | 
| 20200319 | 1\.38\.0 | 18\.09\.9\-ce | 1\.38\.0\-1 | 
| 20200218 | 1\.37\.0 | 18\.09\.9\-ce | 1\.37\.0\-2 | 
| 20200205 | 1\.36\.2 | 18\.09\.9\-ce | 1\.36\.2\-1 | 
| 20200115 | 1\.36\.1 | 18\.09\.9\-ce | 1\.36\.1\-1 | 
| 20200108 | 1\.36\.0 | 18\.09\.9\-ce | 1\.36\.0\-1 | 
| 20191212 | 1\.35\.0 | 18\.09\.9\-ce | 1\.35\.0\-1 | 
| 20191114 | 1\.33\.0 | 18\.06\.1\-ce | 1\.33\.0\-1 | 
| 20191031 | 1\.32\.1 | 18\.06\.1\-ce | 1\.32\.1\-1 | 
| 20191014 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 20190925 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 20190913 | 1\.31\.0 | 18\.06\.1\-ce | 1\.31\.0\-1 | 
| 20190815 | 1\.30\.0 | 18\.06\.1\-ce | 1\.30\.0\-1 | 
| 20190709 | 1\.29\.1 | 18\.06\.1\-ce | 1\.29\.1\-1 | 
| 20190614 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 20190607 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 20190603 | 1\.28\.1 | 18\.06\.1\-ce | 1\.28\.1\-2 | 
| 20190510 | 1\.28\.0 | 18\.06\.1\-ce | 1\.28\.0\-1 | 
| 20190402 | 1\.27\.0 | 18\.06\.1\-ce | 1\.27\.0\-1 | 
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
| 20200603 | 1\.40\.0 | 19\.03\.6\-ce | 1\.40\.0\-1 | 
| 20200430 | 1\.39\.0 | 19\.03\.6\-ce | 1\.39\.0\-1 | 
| 20200402 | 1\.39\.0 | 18\.09\.9\-ce | 1\.39\.0\-1 | 
| 20200319 | 1\.38\.0 | 18\.09\.9\-ce | 1\.38\.0\-1 | 
| 20200218 | 1\.37\.0 | 18\.09\.9\-ce | 1\.37\.0\-2 | 
| 20200205 | 1\.36\.2 | 18\.09\.9\-ce | 1\.36\.2\-1 | 
| 20200115 | 1\.36\.1 | 18\.09\.9\-ce | 1\.36\.1\-1 | 
| 20200108 | 1\.36\.0 | 18\.09\.9\-ce | 1\.36\.0\-1 | 
| 20191212 | 1\.35\.0 | 18\.09\.9\-ce | 1\.35\.0\-1 | 
| 20191114 | 1\.33\.0 | 18\.06\.1\-ce | 1\.33\.0\-1 | 
| 20191031 | 1\.32\.1 | 18\.06\.1\-ce | 1\.32\.1\-1 | 
| 20191014 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 20190925 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 20190913 | 1\.31\.0 | 18\.06\.1\-ce | 1\.31\.0\-1 | 
| 20190815 | 1\.30\.0 | 18\.06\.1\-ce | 1\.30\.0\-1 | 
| 20190709 | 1\.29\.1 | 18\.06\.1\-ce | 1\.29\.1\-1 | 
| 20190617 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 20190607 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 20190603 | 1\.28\.1 | 18\.06\.1\-ce | 1\.28\.1\-2 | 
| 20190510 | 1\.28\.0 | 18\.06\.1\-ce | 1\.28\.0\-1 | 
| 20190403 | 1\.27\.0 | 18\.06\.1\-ce | 1\.27\.0\-1 | 
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

The table below lists the current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, `ecs-init` package, and NVIDIA driver\.


| Amazon ECS GPU\-optimized AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | NVIDIA driver version | 
| --- | --- | --- | --- | --- | 
| 20200603 | 1\.40\.0 | 19\.03\.6\-ce | 1\.40\.0\-1 | 418\.87\.00 | 
| 20200430 | 1\.39\.0 | 19\.03\.6\-ce | 1\.39\.0\-1 | 418\.87\.00 | 
| 20200402 | 1\.39\.0 | 18\.09\.9\-ce | 1\.39\.0\-1 | 418\.87\.00 | 
| 20200319 | 1\.38\.0 | 18\.09\.9\-ce | 1\.38\.0\-1 | 418\.87\.00 | 
| 20200218 | 1\.37\.0 | 18\.09\.9\-ce | 1\.37\.0\-2 | 418\.87\.00 | 
| 20200205 | 1\.36\.2 | 18\.09\.9\-ce | 1\.36\.2\-1 | 418\.87\.00 | 
| 20200115 | 1\.36\.1 | 18\.09\.9\-ce | 1\.36\.1\-1 | 418\.87\.00 | 
| 20200108 | 1\.36\.0 | 18\.09\.9\-ce | 1\.36\.0\-1 | 418\.87\.00 | 
| 20191212 | 1\.35\.0 | 18\.09\.9\-ce | 1\.35\.0\-1 | 418\.87\.00 | 
| 20191114 | 1\.33\.0 | 18\.06\.1\-ce | 1\.33\.0\-1 | 418\.87\.00 | 
| 20191031 | 1\.32\.1 | 18\.06\.1\-ce | 1\.32\.1\-1 | 418\.87\.00 | 
| 20191014 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 418\.87\.00 | 
| 20190925 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 418\.87\.00 | 
| 20190913 | 1\.31\.0 | 18\.06\.1\-ce | 1\.31\.0\-1 | 418\.87\.00 | 
| 20190815 | 1\.30\.0 | 18\.06\.1\-ce | 1\.30\.0\-1 | 418\.40\.04 | 
| 20190709 | 1\.29\.1 | 18\.06\.1\-ce | 1\.29\.1\-1 | 418\.40\.04 | 
| 20190614 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 418\.40\.04 | 
| 20190607 | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 418\.40\.04 | 
| 20190603 | 1\.28\.1 | 18\.06\.1\-ce | 1\.28\.1\-2 | 418\.40\.04 | 
| 20190510 | 1\.28\.0 | 18\.06\.1\-ce | 1\.28\.0\-1 | 418\.40\.04 | 
| 20190402 | 1\.27\.0 | 18\.06\.1\-ce | 1\.27\.0\-1 | 418\.40\.04 | 
| 20190321 | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 410\.104 | 
| 20190301 | 1\.26\.0 | 18\.06\.1\-ce | 1\.26\.0\-1 | 396\.26 | 
| 20190215 | 1\.25\.3 | 18\.06\.1\-ce | 1\.25\.3\-1 | 396\.26 | 
| 20190204 | 1\.25\.2 | 18\.06\.1\-ce | 1\.25\.2\-1 | 396\.26 | 
| 20190127 | 1\.25\.1 | 18\.06\.1\-ce | 1\.25\.1\-1 | 396\.26 | 
| 20190118 | 1\.25\.0 | 18\.06\.1\-ce | 1\.25\.0\-1 | 396\.26 | 

You can retrieve the current Amazon ECS GPU\-optimized AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended
```

## Amazon ECS\-optimized Amazon Linux AMI Versions<a name="al1-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 2018\.03\.20200603 | 1\.40\.0 | 19\.03\.6\-ce | 1\.40\.0\-1 | 
| 2018\.03\.20200430 | 1\.39\.0 | 19\.03\.6\-ce | 1\.39\.0\-1 | 
| 2018\.03\.20200402 | 1\.39\.0 | 18\.09\.9\-ce | 1\.39\.0\-1 | 
| 2018\.03\.20200319 | 1\.38\.0 | 18\.09\.9\-ce | 1\.38\.0\-1 | 
| 2018\.03\.20200218 | 1\.37\.0 | 18\.09\.9\-ce | 1\.37\.0\-2 | 
| 2018\.03\.20200205 | 1\.36\.2 | 18\.09\.9\-ce | 1\.36\.2\-1 | 
| 2018\.03\.20200115 | 1\.36\.1 | 18\.09\.9\-ce | 1\.36\.1\-1 | 
| 2018\.03\.20200108 | 1\.36\.0 | 18\.09\.9\-ce | 1\.36\.0\-1 | 
| 2018\.03\.20200108 | 1\.36\.0 | 18\.09\.9\-ce | 1\.36\.0\-1 | 
| 2018\.03\.20191212 | 1\.35\.0 | 18\.09\.9\-ce | 1\.35\.0\-1 | 
| 2018\.03\.20191114 | 1\.33\.0 | 18\.06\.1\-ce | 1\.33\.0\-1 | 
| 2018\.03\.20191031 | 1\.32\.1 | 18\.06\.1\-ce | 1\.32\.1\-1 | 
| 2018\.03\.20191016 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 2018\.03\.20191014 | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 2018\.03\.y | 1\.32\.0 | 18\.06\.1\-ce | 1\.32\.0\-1 | 
| 2018\.03\.x | 1\.31\.0 | 18\.06\.1\-ce | 1\.31\.0\-1 | 
| 2018\.03\.w | 1\.30\.0 | 18\.06\.1\-ce | 1\.30\.0\-1 | 
| 2018\.03\.v | 1\.29\.1 | 18\.06\.1\-ce | 1\.29\.1\-1 | 
| 2018\.03\.u | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 2018\.03\.t | 1\.29\.0 | 18\.06\.1\-ce | 1\.29\.0\-1 | 
| 2018\.03\.s | 1\.28\.1 | 18\.06\.1\-ce | 1\.28\.1\-2 | 
| 2018\.03\.q | 1\.28\.0 | 18\.06\.1\-ce | 1\.28\.0\-1 | 
| 2018\.03\.p | 1\.27\.0 | 18\.06\.1\-ce | 1\.27\.0\-1 | 
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

You can retrieve the current Amazon ECS\-optimized Amazon Linux AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

## Amazon ECS\-optimized Windows 2019 Full AMI Versions<a name="windows-2019-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows 2019 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows 2019 Full AMI  |  Amazon ECS container agent version  |  Docker version  | 
| --- | --- | --- | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  | 
|  **2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  | 
|  **2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  | 
|  **2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  | 
|  **2019\.05\.10**  |  `1.27.0`  |  `18.09.4`  | 

The current Amazon ECS\-optimized Windows 2019 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
```

## Amazon ECS\-optimized Windows 2019 Core AMI Versions<a name="windows-2019-core-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows 2019 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows 2019 Core AMI  |  Amazon ECS container agent version  |  Docker version  | 
| --- | --- | --- | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  | 

The current Amazon ECS\-optimized Windows 2019 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized
```

## Amazon ECS\-optimized Windows 1909 Core AMI Versions<a name="windows-1909-core-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows 1909 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows 1909 Core AMI  |  Amazon ECS container agent version  |  Docker version  | 
| --- | --- | --- | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  | 

The current Amazon ECS\-optimized Windows 1909 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized
```

## Amazon ECS\-optimized Windows 2016 Full AMI Versions<a name="windows-2016-ami-agent-versions"></a>

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows 2016 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows 2016 Full AMI  |  Amazon ECS container agent version  |  Docker version  | 
| --- | --- | --- | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  | 
|  **2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  | 
|  **2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  | 
|  **2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  | 
|  **2019\.03\.07**  |  `1.26.0`  |  `18.03.1`  | 

The current Amazon ECS\-optimized Windows 2016 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
```