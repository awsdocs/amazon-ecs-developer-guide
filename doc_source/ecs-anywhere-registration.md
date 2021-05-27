# Registering an external instance to a cluster<a name="ecs-anywhere-registration"></a>

Each on\-premise server or virtual machine \(VM\), also referred to as an *external instance*, you register with an Amazon ECS cluster must have the SSM Agent, the Amazon ECS container agent, and Docker installed on it\. In order to register the external instance to an Amazon ECS cluster, it must first be registered as an AWS Systems Manager managed instance\. The Amazon ECS console makes it easy by creating an installation script which includes an Systems Manager activation key and commands to install each of the required agents and Docker\. The installation script is run on your on\-premise server or VM to complete the installation and registration steps\.

**Note**  
Before registering your external instance with the cluster, if there are container agent configuration parameters you want to use you can create the `/etc/ecs/ecs.config` file on your external instance and add them\. This must be done prior to the external instance being registered to a cluster\. An example of when this is used is specifying the `ECS_CONTAINER_INSTANCE_TAGS` parameter to add resource tags to your external instance\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

**To register an external instance \(AWS Management Console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose a cluster to register your external instance to\.

1. Choose the **ECS Instances** tab, then choose **Register external instances**\.

1. On the **Step 1: External instances activation details** page, complete the following steps\.

   1. For **Activation key duration \(in days\)**, enter the number of days the activation key should remain active for\. After this duration, the key will no longer work when registering an External instance\.

   1. For **Number of instances**, enter the number of external instances you want to register to your cluster with the activation key\.

   1. For **Instance role**, choose the IAM role to associate with your external instances\. If a role hasn't been created, choose **Create new role** to have Amazon ECS create a role on your behalf\. For more information on what IAM permissions are required for your external instances, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   1. Choose **Next step**\.

1. On the **Step 2: Register external instances** page, copy the registration command\. This command should be run on each external instance you want to register to the cluster\.
**Important**  
The bash portion of the script must be run as root\. If the command is not run as root, an error is returned\.

The AWS CLI can be used to create a Systems Manager activation before running the installation script to complete the external instance registration process\.

**To register an external instance \(AWS CLI\)**

1. Create an Systems Manager activation pair\. This is used for Systems Manager managed instance activation\. The output will include an `ActivationId` and `ActivationCode` that you use in the next step\. Ensure you specify the ECS Anywhere IAM role you created\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   ```
   aws ssm create-activation --iam-role ECSAnywhereRole | tee ssm-activation.json
   ```

1. On your on\-premise server or virtual machine \(VM\), use the following steps to download and run the installation script\.

   1. Download the installation script\.

      ```
      curl --proto "https" -o "/tmp/ecs-anywhere-install.sh" "https://amazon-ecs-agent.s3.amazonaws.com/ecs-anywhere-install-latest.sh"
      ```

   1. Run the installation script\. Specify the cluster name, Region, and the Systems Manager activation ID and activation code from the first step\.

      ```
      sudo bash /tmp/ecs-anywhere-install.sh --region $REGION --cluster $CLUSTER_NAME --activation-id $ACTIVATION_ID --activation-code $ACTIVATION_CODE
      ```

Use the following steps to register an existing external instance with a different cluster\.

**To register an existing external instance with a different cluster**

1. Stop the Amazon ECS container agent\.

   ```
   sudo systemctl stop ecs.service
   ```

1. Edit the `/ecs/etc/ecs.config` file and on the `ECS_CLUSTER` line, ensure the cluster name matches the name of the cluster to register the external instance with\.

1. Remove the existing Amazon ECS agent data\.

   ```
   sudo rm /var/lib/ecs/data/agent.db
   ```

1. Start the Amazon ECS container agent\.

   ```
   sudo systemctl start ecs.service
   ```