# Registering an external instance to a cluster<a name="ecs-anywhere-registration"></a>

For each external instance you register with an Amazon ECS cluster, it must have the SSM Agent, the Amazon ECS container agent, and Docker installed\. To register the external instance to an Amazon ECS cluster, it must first be registered as an AWS Systems Manager managed instance\. You can create the installation script in a few clicks on the Amazon ECS console\. The installation script includes an Systems Manager activation key and commands to install each of the required agents and Docker\. The installation script must be run on your on\-premises server or VM to complete the installation and registration steps\.

**Note**  
Before registering your Linux external instance with the cluster, create the `/etc/ecs/ecs.config` file on your external instance and add any container agent configuration parameters that you want\. You can't do this after registering the external instance to a cluster\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

------
#### [ New AWS Management Console ]

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose a cluster to register your external instance to\.

1. On the **Cluster : *name*** page, choose the **Infrastructure** tab\.

1. On the **Register external instances** page, complete the following steps\.

   1. For **Activation key duration \(in days\)**, enter the number of days that the activation key remains active for\. After the number of days you entered pass, the key no longer works when registering an external instance\.

   1. For **Number of instances**, enter the number of external instances that you want to register to your cluster with the activation key\.

   1. For **Instance role**, choose the IAM role to associate with your external instances\. If a role wasn't already created, choose **Create new role** to have Amazon ECS create a role on your behalf\. For more information about what IAM permissions are required for your external instances, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   1.  Copy the registration command\. This command should be run on each external instance you want to register to the cluster\.
**Important**  
The bash portion of the script must be run as root\. If the command isn't run as root, an error is returned\.

   1. Choose **Close**\.

------
#### [ AWS CLI for Linux operating systems ]

1. Create an Systems Manager activation pair\. This is used for Systems Manager managed instance activation\. The output includes an `ActivationId` and `ActivationCode`\. You use these in a later step\. Make sure that you specify the ECS Anywhere IAM role that you created\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   ```
   aws ssm create-activation --iam-role ecsAnywhereRole | tee ssm-activation.json
   ```

1. On your on\-premises server or virtual machine \(VM\), download the installation script\.

   ```
   curl --proto "https" -o "/tmp/ecs-anywhere-install.sh" "https://amazon-ecs-agent.s3.amazonaws.com/ecs-anywhere-install-latest.sh"
   ```

1. \(Optional\) On your on\-premises server or virtual machine \(VM\), use the following steps to verify the installation script using the script signature file\.

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

1. On your on\-premises server or virtual machine \(VM\), run the installation script\. Specify the cluster name, Region, and the Systems Manager activation ID and activation code from the first step\.

   ```
   sudo bash /tmp/ecs-anywhere-install.sh \
       --region $REGION \
       --cluster $CLUSTER_NAME \
       --activation-id $ACTIVATION_ID \
       --activation-code $ACTIVATION_CODE
   ```

   For an on\-premises server or virtual machine \(VM\) that has the NVIDIA driver installed for GPU workloads, you must add the `--enable-gpu` flag to the installation script\. When this flag is specified, the install script verifies that the NVIDIA driver is running and then adds the required configuration variables to run your Amazon ECS tasks\. For more information about running GPU workloads and specifying GPU requirements in a task definition, see [Specifying GPUs in your task definition](ecs-gpu.md#ecs-gpu-specifying)\.

   ```
   sudo bash /tmp/ecs-anywhere-install.sh \
       --region $REGION \
       --cluster $CLUSTER_NAME \
       --activation-id $ACTIVATION_ID \
       --activation-code $ACTIVATION_CODE
       --enable-gpu
   ```

Use the following steps to register an existing external instance with a different cluster\.

**To register an existing external instance with a different cluster**

1. Stop the Amazon ECS container agent\.

   ```
   sudo systemctl stop ecs.service
   ```

1. Edit the `/etc/ecs/ecs.config` file and on the `ECS_CLUSTER` line, ensure the cluster name matches the name of the cluster to register the external instance with\.

1. Remove the existing Amazon ECS agent data\.

   ```
   sudo rm /var/lib/ecs/data/agent.db
   ```

1. Start the Amazon ECS container agent\.

   ```
   sudo systemctl start ecs.service
   ```

------
#### [ AWS CLI for Windows operating systems ]

1. Create an Systems Manager activation pair\. This is used for Systems Manager managed instance activation\. The output includes an `ActivationId` and `ActivationCode`\. You use these in a later step\. Make sure that you specify the ECS Anywhere IAM role that you created\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

   ```
   aws ssm create-activation --iam-role ecsAnywhereRole | tee ssm-activation.json
   ```

1. On your on\-premises server or virtual machine \(VM\), download the installation script\.

   ```
   Invoke-RestMethod -URI "https://amazon-ecs-agent.s3.amazonaws.com/ecs-anywhere-install.ps1" -OutFile “ecs-anywhere-install.ps1”
   ```

1. \(Optional\) The Powershell script is signed by Amazon and therefore, Windows automatically performs the certificate validation on the same\. You do not need to perform any manual validation\.

   To manually verify the certificate, right\-click on the file, navigate to properties and use the Digital Signatures tab to obtain more details\.

   This option is only available when the host has the certificate in the certificate store\.

   The verification should return information similar to the following:

   ```
   # Verification (PowerShell)
   Get-AuthenticodeSignature -FilePath .\ecs-anywhere-install.ps1
   
   SignerCertificate                         Status      Path
   -----------------                         ------      ----
   EXAMPLECERTIFICATE                        Valid       ecs-anywhere-install.ps1
   
   ...
   
   Subject              : CN="Amazon Web Services, Inc.",...
   
   ----
   ```

1. On your on\-premises server or virtual machine \(VM\), run the installation script\. Specify the cluster name, Region, and the Systems Manager activation ID and activation code from the first step\.

   ```
   .\ecs-anywhere-install.ps1 -Region $Region -Cluster $Cluster -ActivationID $ActivationID -ActivationCode $ActivationCode
   ```

1. Verify the Amazon ECS container agent is running\.

   ```
   Get-Service AmazonECS
   
   Status   Name               DisplayName
   ------   ----               -----------
   Running  AmazonECS          Amazon ECS
   ```

Use the following steps to register an existing external instance with a different cluster\.

**To register an existing external instance with a different cluster**

1. Stop the Amazon ECS container agent\.

   ```
   Stop-Service AmazonECS
   ```

1. Modify the `ECS_CLUSTER` parameter so that the cluster name matches the name of the cluster to register the external instance with\.

   ```
   [Environment]::SetEnvironmentVariable("ECS_CLUSTER", $ECSCluster, [System.EnvironmentVariableTarget]::Machine)
   ```

1. Remove the existing Amazon ECS agent data\.

   ```
   Remove-Item -Recurse -Force $env:ProgramData\Amazon\ECS\data\*
   ```

1. Start the Amazon ECS container agent\.

   ```
   Start-Service AmazonECS
   ```

------

The AWS CLI can be used to create a Systems Manager activation before running the installation script to complete the external instance registration process\.