# Amazon ECS Container Agent Versions<a name="container_agent_versions"></a>

Each Amazon ECS container agent version supports a different feature set and provides bug fixes from previous versions\. When possible, we always recommend using the latest version of the Amazon ECS container agent\. To update your container agent to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

Launching your container instances from the most recent Amazon ECS\-optimized AMI ensures that you receive the current container agent version\. To launch a container instance with the latest Amazon ECS\-optimized AMI, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

To install the latest version of the Amazon ECS container agent on another operating system, see [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\. The table in [Amazon ECS\-Optimized AMI Container Agent Versions](#ecs-optimized-ami-agent-versions) shows the Docker version that is tested on Amazon Linux for each agent version\.

To see which features and enhancements are included with each agent release, see [https://github\.com/aws/amazon\-ecs\-agent/releases](https://github.com/aws/amazon-ecs-agent/releases)\.

## Amazon ECS\-Optimized AMI Container Agent Versions<a name="ecs-optimized-ami-agent-versions"></a>

The Amazon ECS\-optimized AMI comes prepackaged with the Amazon ECS container agent, Docker, and the `ecs-init` service that controls the starting and stopping of the agent at boot and shutdown\. The following table lists the container agent version, the `ecs-init` version, and the Docker version that is tested and packaged with each Amazon ECS\-optimized AMI\.

**Note**  
As new Amazon ECS\-optimized AMIs and Amazon ECS agent versions are released, older versions are still available for launch in Amazon EC2\. However, we encourage you to update to the latest version of the Amazon ECS agent and to keep your container instance software up to date\. If you request support for an older version of the Amazon ECS agent through AWS Support, you may be asked to move to the latest version as a part of the support process\.


| Amazon ECS\-optimized AMI | Amazon ECS container agent version | Docker version | `ecs-init` version | 
| --- | --- | --- | --- | 
| 2017\.09\.d | 1\.6\.0 | 17\.06\.2\-ce | 1\.16\.0\-1 | 
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

For more information about the Amazon ECS\-optimized AMI, including AMI IDs for the latest version in each region, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\.