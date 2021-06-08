# Registering an external instance to a cluster<a name="ecs-anywhere-registration"></a>

For each external instance you register with an Amazon ECS cluster, it must have the SSM Agent, the Amazon ECS container agent, and Docker installed\. To register the external instance to an Amazon ECS cluster, it must first be registered as an AWS Systems Manager managed instance\. You can create the installation script in a few clicks on the Amazon ECS console\. The installation script includes an Systems Manager activation key and commands to install each of the required agents and Docker\. The installation script must be run on your on\-premises server or VM to complete the installation and registration steps\.

**Note**  
Before registering your external instance with the cluster, create the `/etc/ecs/ecs.config` file on your external instance and add any container agent configuration parameters that you want\. You can't do this after registering the external instance to a cluster\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

**To register an external instance \(AWS Management Console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose a cluster to register your external instance to\.

1. Choose the **ECS Instances** tab, then choose **Register external instances**\.

1. On the **Step 1: External instances activation details** page, complete the following steps\.

   1. For **Activation key duration \(in days\)**, enter the number of days that the activation key remains active for\. After the number of days you entered pass, the key no longer works when registering an external instance\.

   1. For **Number of instances**, enter the number of external instances that you want to register to your cluster with the activation key\.

   1. For **Instance role**, choose the IAM role to associate with your external instances\. If a role wasn't already created, choose **Create new role** to have Amazon ECS create a role on your behalf\. For more information about what IAM permissions are required for your external instances, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   1. Choose **Next step**\.

1. On the **Step 2: Register external instances** page, copy the registration command\. This command should be run on each external instance you want to register to the cluster\.
**Important**  
The bash portion of the script must be run as root\. If the command isn't run as root, an error is returned\.

The AWS CLI can be used to create a Systems Manager activation before running the installation script to complete the external instance registration process\.

**To register an external instance \(AWS CLI\)**

1. Create an Systems Manager activation pair\. This is used for Systems Manager managed instance activation\. The output includes an `ActivationId` and `ActivationCode`\. You use these in the next step\. Make sure that you specify the ECS Anywhere IAM role that you created\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   ```
   aws ssm create-activation --iam-role ECSAnywhereRole | tee ssm-activation.json
   ```

1. On your on\-premises server or virtual machine \(VM\), use the following steps to download and run the installation script\.

   1. Download and install GnuPG\. For more information about GNUpg, see the [GnuPG website](https://www.gnupg.org)\. For Linux systems, install `gpg` using the package manager on your flavor of Linux\.

   1. Retrieve the Amazon ECS PGP public key\.

      ```
      gpg --keyserver hkp://keys.gnupg.net:80 --recv BCE9D9A42D51784F
      ```

   1. Download the installation script signature\. The signature is an ascii detached PGP signature stored in a file with the `.asc` extension\.

      ```
      curl --proto "https" -o "/tmp/ecs-anywhere-install.sh.asc" "https://amazon-ecs-agent.s3.amazonaws.com/ecs-anywhere-install-latest.sh.asc"
      ```

   1. Verify the installation script file using the key\.

      ```
      gpg --verify /tmp/ecs-anywhere-install.sh.asc /tmp/ecs-anywhere-install.sh
      ```

      The following is the expected output\.

      ```
      gpg: Signature made Tue 25 May 2021 07:16:29 PM UTC
      gpg:                using RSA key 50DECCC4710E61AF
      gpg: Good signature from "Amazon ECS <ecs-security@amazon.com>" [unknown]
      gpg: WARNING: This key is not certified with a trusted signature!
      gpg:          There is no indication that the signature belongs to the owner.
      Primary key fingerprint: F34C 3DDA E729 26B0 79BE  AEC6 BCE9 D9A4 2D51 784F
           Subkey fingerprint: D64B B6F9 0CF3 77E9 B5FB  346F 50DE CCC4 710E 61AF
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