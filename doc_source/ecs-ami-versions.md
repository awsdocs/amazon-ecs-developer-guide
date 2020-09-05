# Amazon ECS\-optimized AMI versions<a name="ecs-ami-versions"></a>

This topic lists the current and previous versions of the Amazon ECS\-optimized AMIs and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

## Linux Amazon ECS\-optimized AMIs versions<a name="ecs-ami-versions-linux"></a>

The following tabs display a list of Linux Amazon ECS\-optimized AMIs versions\.

------
#### [ Amazon Linux 2 AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20200905 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200902 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200827 | 1\.44\.2 | 19\.03\.6\-ce | 1\.44\.2\-1 | 
| 20200820 | 1\.44\.1 | 19\.03\.6\-ce | 1\.44\.1\-1 | 
| 20200813 | 1\.44\.0 | 19\.03\.6\-ce | 1\.44\.0\-1 | 
| 20200805 | 1\.43\.0 | 19\.03\.6\-ce | 1\.43\.0\-1 | 
| 20200723 | 1\.42\.0 | 19\.03\.6\-ce | 1\.42\.0\-1 | 
| 20200708 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-2 | 
| 20200706 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-1 | 
| 20200623 | 1\.41\.0 | 19\.03\.6\-ce | 1\.41\.0\-1 | 
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

------
#### [ Amazon Linux 2 \(arm64\) AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20200905 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200902 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200827 | 1\.44\.2 | 19\.03\.6\-ce | 1\.44\.2\-1 | 
| 20200820 | 1\.44\.1 | 19\.03\.6\-ce | 1\.44\.1\-1 | 
| 20200813 | 1\.44\.0 | 19\.03\.6\-ce | 1\.44\.0\-1 | 
| 20200805 | 1\.43\.0 | 19\.03\.6\-ce | 1\.43\.0\-1 | 
| 20200723 | 1\.42\.0 | 19\.03\.6\-ce | 1\.42\.0\-1 | 
| 20200708 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-2 | 
| 20200706 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-1 | 
| 20200623 | 1\.41\.0 | 19\.03\.6\-ce | 1\.41\.0\-1 | 
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

------
#### [ Amazon Linux 2 \(GPU\) AMI versions ]

The table below lists the current and previous versions of the Amazon ECS GPU\-optimized AMI and their corresponding versions of the Amazon ECS container agent, Docker, `ecs-init` package, and NVIDIA driver\.


| Amazon ECS GPU\-optimized AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | NVIDIA driver version | 
| --- | --- | --- | --- | --- | 
| 20200905 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 418\.87\.00 | 
| 20200902 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 418\.87\.00 | 
| 20200827 | 1\.44\.2 | 19\.03\.6\-ce | 1\.44\.2\-1 | 418\.87\.00 | 
| 20200820 | 1\.44\.1 | 19\.03\.6\-ce | 1\.44\.1\-1 | 418\.87\.00 | 
| 20200813 | 1\.44\.0 | 19\.03\.6\-ce | 1\.44\.0\-1 | 418\.87\.00 | 
| 20200805 | 1\.43\.0 | 19\.03\.6\-ce | 1\.43\.0\-1 | 418\.87\.00 | 
| 20200723 | 1\.42\.0 | 19\.03\.6\-ce | 1\.42\.0\-1 | 418\.87\.00 | 
| 20200708 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-2 | 418\.87\.00 | 
| 20200706 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-1 | 418\.87\.00 | 
| 20200623 | 1\.41\.0 | 19\.03\.6\-ce | 1\.41\.0\-1 | 418\.87\.00 | 
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

------
#### [ Amazon Linux 2 \(Inferentia\) AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 20200905 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200902 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 20200827 | 1\.44\.2 | 19\.03\.6\-ce | 1\.44\.2\-1 | 
| 20200820 | 1\.44\.1 | 19\.03\.6\-ce | 1\.44\.1\-1 | 
| 20200813 | 1\.44\.0 | 19\.03\.6\-ce | 1\.44\.0\-1 | 
| 20200805 | 1\.43\.0 | 19\.03\.6\-ce | 1\.43\.0\-1 | 
| 20200723 | 1\.42\.0 | 19\.03\.6\-ce | 1\.42\.0\-1 | 
| 20200708 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-2 | 
| 20200706 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-1 | 
| 20200623 | 1\.41\.0 | 19\.03\.6\-ce | 1\.41\.0\-1 | 

You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

------
#### [ Amazon Linux AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Amazon Linux AMI and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.


| Amazon ECS\-optimized Amazon Linux AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 2018\.03\.20200905 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 2018\.03\.20200902 | 1\.44\.3 | 19\.03\.6\-ce | 1\.44\.3\-1 | 
| 2018\.03\.20200827 | 1\.44\.2 | 19\.03\.6\-ce | 1\.44\.2\-1 | 
| 2018\.03\.20200820 | 1\.44\.1 | 19\.03\.6\-ce | 1\.44\.1\-1 | 
| 2018\.03\.20200813 | 1\.44\.0 | 19\.03\.6\-ce | 1\.44\.0\-1 | 
| 2018\.03\.20200805 | 1\.43\.0 | 19\.03\.6\-ce | 1\.43\.0\-1 | 
| 2018\.03\.20200723 | 1\.42\.0 | 19\.03\.6\-ce | 1\.42\.0\-1 | 
| 2018\.03\.20200708 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-2 | 
| 2018\.03\.20200706 | 1\.41\.1 | 19\.03\.6\-ce | 1\.41\.1\-1 | 
| 2018\.03\.20200623 | 1\.41\.0 | 19\.03\.6\-ce | 1\.41\.0\-1 | 
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

------

## Windows Amazon ECS\-optimized AMIs versions<a name="ecs-ami-versions-windows"></a>

The following tabs display a list of Windows Amazon ECS\-optimized AMIs versions\.

**Important**  
To ensure that customers have the latest security updates by default, Amazon ECS maintains at least the last three Windows Amazon ECS\-optimized AMIs\. After releasing new Windows Amazon ECS\-optimized AMIs, Amazon ECS makes the Windows Amazon ECS\-optimized AMIs that are older private\. If there is a private AMI that you need access to, let us know by filing a ticket with Cloud Support\.

------
#### [ Windows Server 2019 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2019 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2019 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Public  | 
|  **2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Public  | 
|  **2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Public  | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Public  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 
|  **2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  |  Private  | 
|  **2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  |  Private  | 
|  **2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  |  Private  | 
|  **2019\.05\.10**  |  `1.27.0`  |  `18.09.4`  |  Private  | 

The current Amazon ECS\-optimized Windows Server 2019 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
```

------
#### [ Windows Server 2019 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2019 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2019 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Public  | 
|  **2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Public  | 
|  **2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Public  | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Public  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 

The current Amazon ECS\-optimized Windows Server 2019 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized
```

------
#### [ Windows Server 1909 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 1909 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 1909 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Public  | 
|  **2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Public  | 
|  **2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Public  | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Public  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 

The current Amazon ECS\-optimized Windows Server 1909 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-1909-English-Core-ECS_Optimized
```

------
#### [ Windows Server 2016 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2016 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2016 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Public  | 
|  **2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Public  | 
|  **2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Public  | 
|  **2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Public  | 
|  **2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 
|  **2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  |  Private  | 
|  **2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  |  Private  | 
|  **2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  |  Private  | 
|  **2019\.03\.07**  |  `1.26.0`  |  `18.03.1`  |  Private  | 

The current Amazon ECS\-optimized Windows Server 2016 Full AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
```

------
#### [ Windows Server 2004 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2004 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2004 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Public  | 

The current Amazon ECS\-optimized Windows Server 2004 Core AMI can be retrieved using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized
```

------