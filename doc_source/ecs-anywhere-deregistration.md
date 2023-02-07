# Deregistering an external instance<a name="ecs-anywhere-deregistration"></a>

We recommend that, after you finish using an external instance, you deregister the instance from both Amazon ECS and AWS Systems Manager\. Following deregistration, the external instance is no longer able to accept new tasks\.

If you have tasks that are running on the container instance when you deregister it, the tasks remain running until they stop through some other means\. However, these tasks are no longer monitored or accounted for by Amazon ECS\. If these tasks on your external instance are part of an Amazon ECS service, then the service scheduler starts another copy of that task, on a different instance, if possible\.

To register an external instance to a new cluster, after the external instance has been deregistered from both Amazon ECS and Systems Manager, you can clean up the remaining AWS resources on the instance and register it with a new cluster\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, choose the Region where your external instance is registered\.

1. In the navigation pane, choose **Clusters** and select the cluster that hosts the external instance\.

1. On the **Cluster : *name*** page, choose the **Infrastructure** tab\.

1. Under **Container instances**, select the external instance ID to deregister\. You're redirected to the container instance detail page\.

1. On the **Container Instance : *id*** page, choose **Deregister**\.

1. Review the deregistration message\. Select **Deregister from AWS Systems Manager** to also deregister the external instance as an Systems Manager managed instance\. Choose **Deregister**\.
**Note**  
You can deregister the external instance as an Systems Manager managed instance in the Systems Manager console\. For instructions, see [Deregistering managed instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managed-instances-advanced-deregister.html) in the *AWS Systems Manager User Guide*\.

After you deregister the instance, clean up AWS resources on your on\-premises server or VM \.

------
#### [ Linux operating system ]

1. Make sure that the external instance is deregistered from both Amazon ECS and Systems Manager\.

1. Stop the Amazon ECS container agent and the SSM Agent services on the instance\.

   ```
   sudo systemctl stop ecs amazon-ssm-agent
   ```

1. Remove the Amazon ECS and Systems Manager packages\.

   **For CentOS 7, CentOS 8, and RHEL 7**

   ```
   sudo yum remove -y amazon-ecs-init amazon-ssm-agent
   ```

   **For SUSE Enterprise Server 15**

   ```
   sudo zypper remove -y amazon-ecs-init amazon-ssm-agent
   ```

   **For Debian and Ubuntu**

   ```
   sudo apt remove -y amazon-ecs-init amazon-ssm-agent
   ```

1. Remove the Amazon ECS and Systems Manager files\.

   ```
   sudo rm -rf /var/lib/ecs /etc/ecs /var/lib/amazon/ssm /var/log/ecs /var/log/amazon/ssm
   ```

------
#### [ Windows operating system ]

1. Make sure that the external instance is deregistered from both Amazon ECS and Systems Manager\.

1. Stop the Amazon ECS container agent and the SSM Agent services on the instance\.

   ```
   Stop-Service AmazonECS
   ```

   ```
   Stop-Service AmazonSSMAgent
   ```

1. Remove the Amazon ECS package\.

   ```
   .\ecs-anywhere-install.ps1 -Uninstall
   ```

------