# Amazon ECS Container Agent Versions<a name="ecs-agent-versions"></a>

Each Amazon ECS container agent version supports a different feature set and provides bug fixes from previous versions\. When possible, we always recommend using the latest version of the Amazon ECS container agent\. To update your container agent to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

Launching your container instances from the most recent Amazon ECS\-optimized Amazon Linux 2 AMI ensures that you receive the current container agent version\. To launch a container instance with the latest Amazon ECS\-optimized Amazon Linux 2 AMI, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

To install the latest version of the Amazon ECS container agent on another operating system, see [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\. The table in [Linux Amazon ECS\-optimized AMIs versions](ecs-ami-versions.md#ecs-ami-versions-linux) shows the Docker version that is tested on Amazon Linux 2 for each agent version\. The table in [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](#ecs-optimized-ami-agent-versions) shows the Docker version that is tested on the Amazon Linux AMI for each agent version\.

To see which features and enhancements are included with each agent release, see [https://github\.com/aws/amazon\-ecs\-agent/releases](https://github.com/aws/amazon-ecs-agent/releases)\.

## Amazon ECS\-Optimized Amazon Linux 2 AMI Container Agent Versions<a name="al2-optimized-ami-agent-versions"></a>

The Amazon ECS\-optimized Amazon Linux 2 AMI comes prepackaged with the Amazon ECS container agent, Docker, and the `ecs-init` `systemd` service that controls the starting and stopping of the agent at boot and shutdown\. The following table lists the container agent version, the `ecs-init` version, and the Docker version that is tested and packaged with each Amazon ECS\-optimized Amazon Linux 2 AMI\.

**Note**  
As new Amazon ECS\-optimized Amazon Linux 2 AMIs and Amazon ECS agent versions are released, older versions are still available for launch in Amazon EC2\. However, we encourage you to [update to the latest version](ecs-agent-update.md) of the Amazon ECS agent and to keep your container instance software up to date\. If you request support for an older version of the Amazon ECS agent through AWS Support, you may be asked to move to the latest version as a part of the support process\.

**Important**  
Amazon ECS agent versions 1\.20\.0 and later have deprecated support for Docker versions older than 1\.9\.0\.


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

For more information about the Amazon ECS\-optimized Amazon Linux 2 AMI, including AMI IDs for the latest version in each Region, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

## Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions<a name="ecs-optimized-ami-agent-versions"></a>

The Amazon ECS\-optimized Amazon Linux AMI comes prepackaged with the Amazon ECS container agent, Docker, and the `ecs-init` service that controls the starting and stopping of the agent at boot and shutdown\. The following table lists the container agent version, the `ecs-init` version, and the Docker version that is tested and packaged with each Amazon ECS\-optimized AMI\.

**Note**  
As new Amazon ECS\-optimized Amazon Linux AMIs and Amazon ECS agent versions are released, older versions are still available for launch in Amazon EC2\. However, we encourage you to [update to the latest version](ecs-agent-update.md) of the Amazon ECS agent and to keep your container instance software up\-to\-date\. If you request support for an older version of the Amazon ECS agent through AWS Support, you may be asked to move to the latest version as a part of the support process\.

**Important**  
Amazon ECS agent versions 1\.20\.0 and later have deprecated support for Docker versions older than 1\.9\.0\.


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

For more information about the Amazon ECS\-optimized Amazon Linux AMI, including AMI IDs for the latest version in each Region, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.