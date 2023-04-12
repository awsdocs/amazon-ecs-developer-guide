# Registering an external instance to a cluster using the classic console<a name="ecs-anywhere-registration-classic-console"></a>


|  | 
| --- |
| The new experience is now the default in the Amazon ECS console\. For more information, see [Registering an external instance to a cluster](ecs-anywhere-registration.md)\. | 

For each external instance you register with an Amazon ECS cluster, it must have the SSM Agent, the Amazon ECS container agent, and Docker installed\. To register the external instance to an Amazon ECS cluster, it must first be registered as an AWS Systems Manager managed instance\. You can create the installation script in a few clicks on the Amazon ECS console\. The installation script includes an Systems Manager activation key and commands to install each of the required agents and Docker\. The installation script must be run on your on\-premises server or VM to complete the installation and registration steps\.

**Note**  
Before registering your Linux external instance with the cluster, create the `/etc/ecs/ecs.config` file on your external instance and add any container agent configuration parameters that you want\. You can't do this after registering the external instance to a cluster\.

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose a cluster to register your external instance to\.

1. Choose the **ECS Instances** tab, then choose **Register external instances**\.

1. On the **Step 1: External instances activation details** page, complete the following steps\.

   1. For **Activation key duration \(in days\)**, enter the number of days that the activation key remains active for\. After the number of days you entered pass, the key no longer works when registering an external instance\.

   1. For **Number of instances**, enter the number of external instances that you want to register to your cluster with the activation key\.

   1. For **Instance role**, choose the IAM role to associate with your external instances\. If a role wasn't already created, choose **Create new role** to have Amazon ECS create a role on your behalf\. 

   1. Choose **Next step**\.

1. On the **Step 2: Register external instances** page, copy the registration command\. This command should be run on each external instance you want to register to the cluster\.
**Important**  
The bash portion of the script must be run as root\. If the command isn't run as root, an error is returned\.