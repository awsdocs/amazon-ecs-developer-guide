# Updating the Amazon ECS container agent<a name="ecs-agent-update"></a>

Occasionally, you may need to update the Amazon ECS container agent to pick up bug fixes and new features\. Updating the Amazon ECS container agent does not interrupt running tasks or services on the container instance\. The process for updating the agent differs depending on whether your container instance was launched with an Amazon ECS\-optimized AMI or another operating system\.

**Note**  
Agent updates do not apply to Windows container instances\. We recommend that you launch new container instances to update the agent version in your Windows clusters\.

**Topics**
+ [Checking the Amazon ECS container agent version](#checking_agent_version)
+ [Updating the Amazon ECS container agent on an Amazon ECS\-optimized AMI](agent-update-ecs-ami.md)
+ [Manually updating the Amazon ECS container agent \(for non\-Amazon ECS\-Optimized AMIs\)](manually_update_agent.md)

## Checking the Amazon ECS container agent version<a name="checking_agent_version"></a>

You can check the version of the container agent that is running on your container instances to see if you need to update it\. The container instance view in the Amazon ECS console provides the agent version\. Use the following procedure to check your agent version\.

------
#### [ New AWS Management Console ]

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, choose the Region where your external instance is registered\.

1. In the navigation pane, choose **Clusters** and select the cluster that hosts the external instance\.

1. On the **Cluster : *name*** page, choose the **Infrastructure** tab\.

1. Under **Container instances**, note the **Agent version** column for your container instances\. If the container instance does not contain the latest version of the container agent, the console alerts you with a message and flags the outdated agent version\.

   If your agent version is outdated, you can update your container agent with the following procedures:
   + If your container instance is running an Amazon ECS\-optimized AMI, see [Updating the Amazon ECS container agent on an Amazon ECS\-optimized AMI](agent-update-ecs-ami.md)\.
   + If your container instance is not running an Amazon ECS\-optimized AMI, see [Manually updating the Amazon ECS container agent \(for non\-Amazon ECS\-Optimized AMIs\)](manually_update_agent.md)\.
**Important**  
To update the Amazon ECS agent version from versions before v1\.0\.0 on your Amazon ECS\-optimized AMI, we recommend that you terminate your current container instance and launch a new instance with the most recent AMI version\. Any container instances that use a preview version should be retired and replaced with the most recent AMI\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

------
#### [ Classic AWS Management Console ]

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster that hosts the container instance or instances to check\.

1. On the **Cluster : *cluster\_name*** page, choose **Amazon ECS Instances**\.

1. Note the **Agent version** column for your container instances\. If the container instance does not contain the latest version of the container agent, the console alerts you with a message and flags the outdated agent version\.

   If your agent version is outdated, you can update your container agent with the following procedures:
   + If your container instance is running an Amazon ECS\-optimized AMI, see [Updating the Amazon ECS container agent on an Amazon ECS\-optimized AMI](agent-update-ecs-ami.md)\.
   + If your container instance is not running an Amazon ECS\-optimized AMI, see [Manually updating the Amazon ECS container agent \(for non\-Amazon ECS\-Optimized AMIs\)](manually_update_agent.md)\.
**Important**  
To update the Amazon ECS agent version from versions before v1\.0\.0 on your Amazon ECS\-optimized AMI, we recommend that you terminate your current container instance and launch a new instance with the most recent AMI version\. Any container instances that use a preview version should be retired and replaced with the most recent AMI\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

------
#### [ Amazon ECS container agent introspection API  ]

You can also use the to check the agent Amazon ECS container agent introspection API version from the container instance itself\. For more information, see [Amazon ECS container agent introspection](ecs-agent-introspection.md)\.

**To check if your Amazon ECS container agent is running the latest version with the introspection API**

1. Log in to your container instance via SSH\.

1. Query the introspection API\.

   ```
   [ec2-user ~]$ curl -s 127.0.0.1:51678/v1/metadata | python -mjson.tool
   ```
**Note**  
The introspection API added `Version` information in the version v1\.0\.0 of the Amazon ECS container agent\. If `Version` is not present when querying the introspection API, or the introspection API is not present in your agent at all, then the version you are running is v0\.0\.3 or earlier\. You should update your version\.

------