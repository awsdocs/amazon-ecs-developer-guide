# Getting Started with Windows Containers<a name="ECS_Windows_getting_started"></a>

This tutorial walks you through manually getting Windows containers running on Amazon ECS\. You create a cluster for your Windows container instances, launch one or more container instances into your cluster, register a task definition that uses a Windows container image, create a service that uses that task definition, and then view the sample webpage that the container runs\.

**Topics**
+ [Step 1: Create a Windows Cluster](#create_windows_cluster)
+ [Step 2: Launching a Windows Container Instance into your Cluster](#launch_windows_container_instance)
+ [Step 3 \(Optional\): Install the Amazon ECS Container Agent](#register_windows_task_def)
+ [Step 4: Register a Windows Task Definition](#register_windows_task_def)
+ [Step 5: Create a Service with Your Task Definition](#create_windows_service)
+ [Step 6: View Your Service](#view_windows_service)

## Step 1: Create a Windows Cluster<a name="create_windows_cluster"></a>

You should create a new cluster for your Windows containers\. Linux container instances cannot run Windows containers, and vice versa, so proper task placement is best accomplished by running Windows and Linux container instances in separate clusters\. In this tutorial, you create a cluster called `windows` for your Windows containers\.

**To create a cluster with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. Choose **EC2 Windows \+ Networking** and choose **Next step**\.

1. For **Cluster name** enter a name for your cluster \(in this example, `windows` is the name of the cluster\)\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. Choose **Create an empty cluster**, **Create**\.

**To create a cluster with the AWS CLI**
+ You can create a cluster using the AWS CLI with the following command:

  ```
  aws ecs create-cluster --cluster-name windows
  ```

## Step 2: Launching a Windows Container Instance into your Cluster<a name="launch_windows_container_instance"></a>

You can launch a Windows container instance using the AWS Management Console, as described in this topic\. Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\. After you've launched your instance, you can use it to run tasks\.

**To launch a Windows container instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the region to use\.

1. From the console dashboard, choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, you have the option to choose either the Windows Amazon ECS\-optimized AMI \(recommended\) or another Windows AMI that supports the Amazon ECS specification\.

   1. Option 1: Use the Windows Amazon ECS\-optimized AMI\. Choose **Community AMIs**\.

      Type **ECS\_Optimized** in the **Search community AMIs** field and press the **Enter** key\. Choose **Select** next to the **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2018\.07\.25** AMI\. 

      The current Amazon ECS\-optimized Windows AMI IDs by region are listed below for reference\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_Windows_getting_started.html)

   1. Option 2: Use another Windows AMI\. Choose **Select** next to the Windows AMI of your choice\.

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. The `t2.micro` instance type is selected by default\. The instance type that you select determines the resources available for your tasks to run on\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, set the **Auto\-assign Public IP** check box depending on whether to make your instance accessible from the public Internet\. If your instance should be accessible from the Internet, verify that the **Auto\-assign Public IP** field is set to **Enable**\. If your instance should not be accessible from the Internet, choose **Disable**\.
**Note**  
Container instances need external network access to communicate with the Amazon ECS service endpoint, so if your container instances do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) in the *Amazon VPC User Guide* and [HTTP Proxy Configuration](http_proxy_config.md) in this guide\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

1. On the **Configure Instance Details** page, select the `ecsInstanceRole` **IAM role** value that you created for your container instances in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
**Important**  
If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent will not connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. <a name="windows-instance-launch-user-data-step"></a>If you are using the Amazon ECS\-optimized AMI, expand the **Advanced Details** section and paste the provided user data PowerShell script into the **User data** field\. By default, this script registers your container instance into the `windows` cluster that you created earlier\. To launch into another cluster instead of `windows`, replace the red text in the script below with the name of your cluster\.

   If you are not using the Amazon ECS\-optimized AMI skip to the next step\.
**Note**  
The `-EnableTaskIAMRole` option is required to enable IAM roles for tasks\. For more information, see [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)\.

   ```
   <powershell>
   Import-Module ECSTools
   Initialize-ECSAgent -Cluster 'windows' -EnableTaskIAMRole
   </powershell>
   ```

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, configure the storage for your container instance\. The Windows OS and container images are large \(approximately 9 GiB for the Windows server core base layers\), and just a few images and containers quickly fill up the default 50\-GiB volume size for the Amazon ECS\-optimized Windows AMI\. A larger root volume size \(for example, 200 GiB\) allows for more containers and images on your instance\.

   You can optionally increase or decrease the volume size for your instance to meet your application needs\.

1. Choose **Review and Launch**\.

1. On the **Review Instance Launch** page, under **Security Groups**, you see that the wizard created and selected a security group for you\. By default, you should have port 3389 for RDP connectivity\. To have your containers to receive inbound traffic from the internet, open those ports as well\.

   1. Choose **Edit security groups**\.

   1. On the **Configure Security Group** page, ensure that the **Create a new security group** option is selected\.

   1. Add rules for any other ports that your containers may need and choose **Review and Launch**\. The sample task definition later in this walk through uses port 8080, so you should open that to **Anywhere**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, then select the key pair that you created when getting set up\. 

   When you are ready, select the acknowledgment field, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running`, and it receives a public DNS name\. \(If the **Public DNS** column is hidden, choose the **Show/Hide** icon and choose **Public DNS**\.\)

1. After your instance has launched, you can view your cluster in the Amazon ECS console to see that your container instance has registered with it\.
**Note**  
It can take up to 15 minutes for your Windows container instance to register with your cluster\.

## Step 3 \(Optional\): Install the Amazon ECS Container Agent<a name="register_windows_task_def"></a>

If you used an AMI other than the Amazon ECS\-optimized AMI when creating your Windows instance you must manually install the Amazon ECS container agent\.

The latest Amazon ECS container agent files, by region, are listed below for reference\.


| Region | Region Name | Container agent | Container agent signature | 
| --- | --- | --- | --- | 
| us\-east\-2 | US East \(Ohio\) | [ECS container agent](http://s3-us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/ecs-agent-windows-latest-zip.asc) | 
| us\-east\-1 | US East \(N\. Virginia\) | [ECS container agent](http://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-windows-latest-zip.asc) | 
| us\-west\-2 | US West \(Oregon\) | [ECS container agent](http://s3-us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/ecs-agent-windows-latest-zip.asc) | 
| us\-west\-1 | US West \(N\. California\) | [ECS container agent](http://s3-us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/ecs-agent-windows-latest-zip.asc) | 
| eu\-west\-3 | EU \(Paris\) | [ECS container agent](http://s3-eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/ecs-agent-windows-latest-zip.asc) | 
| eu\-west\-2 | EU \(London\) | [ECS container agent](http://s3-eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/ecs-agent-windows-latest-zip.asc) | 
| eu\-west\-1 | EU \(Ireland\) | [ECS container agent](http://s3-eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/ecs-agent-windows-latest-zip.asc) | 
| eu\-central\-1 | EU \(Frankfurt\) | [ECS container agent](http://s3-eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/ecs-agent-windows-latest-zip.asc) | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) | [ECS container agent](http://s3-ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/ecs-agent-windows-latest-zip.asc) | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) | [ECS container agent](http://s3-ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/ecs-agent-windows-latest-zip.asc) | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) | [ECS container agent](http://s3-ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/ecs-agent-windows-latest-zip.asc) | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) | [ECS container agent](http://s3-ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/ecs-agent-windows-latest-zip.asc) | 
| ca\-central\-1 | Canada \(Central\) | [ECS container agent](http://s3-ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/ecs-agent-windows-latest-zip.asc) | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) | [ECS container agent](http://s3-ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/ecs-agent-windows-latest-zip.asc) | 
| sa\-east\-1 | South America \(São Paulo\) | [ECS container agent](http://s3-sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/ecs-agent-windows-latest-zip.asc) | 
| us\-gov\-west\-1 | AWS GovCloud \(US\) | [ECS container agent](http://s3-us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3-us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/ecs-agent-windows-latest-zip.asc) | 
| cn\-north\-1 | China \(Beijing\) | [ECS container agent](http://s3.cn-north-1.amazonaws.com.cn/amazon-ecs-agent-cn-north-1/ecs-agent-windows-latest.zip) | [PGP signature](http://s3.cn-north-1.amazonaws.com.cn/amazon-ecs-agent-cn-north-1/ecs-agent-windows-latest-zip.asc) | 

**To manually install the Amazon ECS container agent on an instance**

1. Connect to your instance\.

1. Install Docker and run it as a service\. For more information, see [Install Docker Enterprise Edition for Windows Server](https://docs.docker.com/install/windows/docker-ee/)\.

1. The Amazon ECS container agent can be run either as a service or a process\.

   1. Option 1: Run the container agent as a service\. Open PowerShell and run the following set of commands\. Replace the `$agentZipUri` with a region\-specific URL\.

      ```
      # Set up the file directories the Amazon ECS container agent will use.
      PS C:\> New-Item -Type directory -Path ${env:ProgramFiles}\Amazon\ECS -Force
      PS C:\> New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS -Force
      PS C:\> New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS\data -Force
      
      # Set up the configuration.
      PS C:\> $ecsExeDir = "${env:ProgramFiles}\Amazon\ECS"
      
      # Replace "windows" in the following command with your own cluster name
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_CLUSTER", "windows", "Machine")
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_LOGFILE", "${env:ProgramData}\Amazon\ECS\log\ecs-agent.log", "Machine")
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_DATADIR", "${env:ProgramData}\Amazon\ECS\data", "Machine")
      
      # Download the container agent.
      # Replace the "agentVersion" with the version of the container agent you want to download. For example, "latest" will download the most current stable version.
      PS C:\> $agentVersion = "latest"
      
      # Replace the "agentZipUri" with the URL of the regional bucket you want to download the Amazon ECS container agent from.
      PS C:\> $agentZipUri = "http://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-windows-$agentVersion.zip" 
      PS C:\> $zipFile = "${env:TEMP}\ecs-agent.zip"
      PS C:\> Invoke-RestMethod -OutFile $zipFile -Uri $agentZipUri
      
      # Put the executables in the executable directory.
      PS C:\> Expand-Archive -Path $zipFile -DestinationPath $ecsExeDir -Force
      PS C:\> Set-Location ${ecsExeDir}
      
      # Set $EnableTaskIAMRoles to $true to enable task IAM roles.
      # Note that enabling IAM roles will make port 80 unavailable for tasks.
      PS C:\> [bool]$EnableTaskIAMRoles = $false
      PS C:\> if (${EnableTaskIAMRoles}) {.\hostsetup.ps1}
      
      # Install the container agent service.
      PS C:\> New-Service -Name "AmazonECS" -BinaryPathName "$ecsExeDir\amazon-ecs-agent.exe -windows-service" -DisplayName "Amazon ECS" -Description "Amazon ECS service runs the Amazon ECS agent" -DependsOn Docker -StartupType Manual
      PS C:\> sc.exe failure AmazonECS reset=300 actions=restart/5000/restart/30000/restart/60000
      PS C:\> sc.exe failureflag AmazonECS 1
      
      # Start the AmazonECS service.
      PS C:\> Start-Service AmazonECS
      ```

   1. Option 2: Run the container agent as a process\. Open PowerShell and run the following set of commands\.

      ```
      PS C:\> # Set up directories the agent uses
      PS C:\> New-Item -Type directory -Path ${env:ProgramFiles}\Amazon\ECS -Force
      PS C:\> New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS -Force
      PS C:\> New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS\data -Force
      
      PS C:\> # Set up configuration
      PS C:\> $ecsExeDir = "${env:ProgramFiles}\Amazon\ECS"
      
      PS C:\> # Replace "windows" in the following command with your own cluster name
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_CLUSTER", "windows", "Machine")
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_LOGFILE", "${env:ProgramData}\Amazon\ECS\log\ecs-agent.log", "Machine")
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_DATADIR", "${env:ProgramData}\Amazon\ECS\data", "Machine")
      
      PS C:\> # Set this environment variable to "true" to enable IAM roles.  Note that enabling IAM roles will make port 80 unavailable for tasks.
      PS C:\> [Environment]::SetEnvironmentVariable("ECS_ENABLE_TASK_IAM_ROLE", "false", "Machine")
      
      # Download the container agent.
      # Replace the "agentVersion" with the version of the container agent you want to download. For example, "latest" will download the most current stable version.
      PS C:\> $agentVersion = "latest"
      
      # Replace the "agentZipUri" with the URL of the regional bucket you want to download the Amazon ECS container agent from.
      PS C:\> $agentZipUri = "http://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-windows-$agentVersion.zip"
      PS C:\> $zipFile = "${env:TEMP}\ecs-agent.zip"
      PS C:\> Invoke-RestMethod -OutFile $zipFile -Uri $agentZipUri
      
      PS C:\> # Put the executables in the executable directory.
      PS C:\> Expand-Archive -Path $zipFile -DestinationPath $ecsExeDir -Force
      
      PS C:\> # Run the container agent.
      PS C:\> cd '$ecsExeDir'; .\amazon-ecs-agent.ps1
      ```
**Note**  
If you reboot the Windows instance, the files in the `${env:TEMP}` directory will be removed\. If this occurs, you will need to download the zip file again\.

1. \(Optional\) You can verify the validity of the container agent file using the Windows binary and PGP signatures\.

   1. Get the Windows binary signature\.

      ```
      PS C:\> $sig=Get-AuthenticodeSignature -FilePath ${env:ProgramFiles}\Amazon\ECS\amazon-ecs-agent.exe
      ```

   1. Verify the Windows binary signature\.

      ```
      PS C:\> $sig.Status
      ```

      Expected output:

      ```
      Valid
      ```

      ```
      PS C:\> $sig.SignerCertificate.Subject
      ```

      Expected output:

      ```
      CN=Amazon Services LLC, OU=Software Services, O=Amazon Services LLC, L=Seattle, S=Washington, C=US
      ```

   1. Download and install GnuPG\. For Windows, download and use the Windows simple installer from the GnuPG website\. For more information about GNUpg, see the [GnuPG website](https://www.gnupg.org)\.

   1. Retrieve the Amazon ECS PGP public key\. You can use a command to do this or manually create the key and then import it\.

      1. Option 1: Retrieve the key with the following command\.

         ```
         gpg --keyserver hkp://keys.gnupg.net --recv BCE9D9A42D51784F
         ```

      1. Option 2: Create a file with the following contents of the Amazon ECS PGP public key and then import it:

         ```
         -----BEGIN PGP PUBLIC KEY BLOCK-----
         Version: GnuPG v2
         
         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
         =Gi5D
         -----END PGP PUBLIC KEY BLOCK-----
         ```

         The details of the Amazon ECS PGP public key for reference:

         ```
         Key ID: BCE9D9A42D51784F
         Type: RSA
         Size: 4096/4096
         Expires: Never
         User ID: Amazon ECS
         Key fingerprint: F34C 3DDA E729 26B0 79BE AEC6 BCE9 D9A4 2D51 784F
         ```

         Import the Amazon ECS PGP public key with the following command\.

         ```
         gpg --import <public_key_filename>
         ```

   1. Download the ECS container agent signature\. ECS container agent signatures are ascii detached PGP signatures stored in files with the extension `.asc`\. The signatures file has the same name as its corresponding executable, with `.asc` appended\.

      ```
      PS C:\> Invoke-WebRequest -OutFile ecs-agent.asc http://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-windows-latest-zip.asc
      ```

   1. Verify the signature\.

      ```
      gpg --verify ecs-agent.asc ${env:TEMP}\ecs-agent.zip
      ```

      Expected output:

      ```
      gpg: Signature made Wed 16 May 2018 08:21:06 PM UTC using RSA key ID 710E61AF
      gpg: Good signature from "Amazon ECS <ecs-security@amazon.com>" [unknown]
      gpg: WARNING: This key is not certified with a trusted signature!
      gpg:          There is no indication that the signature belongs to the owner.
      Primary key fingerprint: F34C 3DDA E729 26B0 79BE  AEC6 BCE9 D9A4 2D51 784F
           Subkey fingerprint: D64B B6F9 0CF3 77E9 B5FB  346F 50DE CCC4 710E 61AF
      ```
**Note**  
The warning in the output is expected and is not problematic; it occurs because there is not a chain of trust between your personal PGP key \(if you have one\) and the Amazon ECS PGP key\. For more information, see [Web of trust](https://en.wikipedia.org/wiki/Web_of_trust)\.

## Step 4: Register a Windows Task Definition<a name="register_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple webpage on port 8080 of a container instance with the `microsoft/iis` container image\.

**To register the sample task definition with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. Scroll to the bottom of the page and choose **Configure via JSON**\.

1. Paste the sample task definition JSON below into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.

   ```
   {
     "family": "windows-simple-iis",
     "containerDefinitions": [
       {
         "name": "windows_sample_app",
         "image": "microsoft/iis",
         "cpu": 100,
         "entryPoint":["powershell", "-Command"],
         "command":["New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"],
         "portMappings": [
           {
             "protocol": "tcp",
             "containerPort": 80,
             "hostPort": 8080
           }
         ],
         "memory": 500,
         "essential": true
       }
     ]
   }
   ```

1. Verify your information and choose **Create**\.

**To register the sample task definition with the AWS CLI**

1. Create a file called `windows-simple-iis.json`\.

1. Open the file with your favorite text editor and add the sample JSON above to the file and save it\.

1. Using the AWS CLI, run the following command to register the task definition with Amazon ECS\.
**Note**  
Make sure that your AWS CLI is configured to use the same region that your Windows cluster exists in, or add the `--region your_cluster_region` option to your command\.

   ```
   aws ecs register-task-definition --cli-input-json file://windows-simple-iis.json
   ```

## Step 5: Create a Service with Your Task Definition<a name="create_windows_service"></a>

After you have registered your task definition, you can place tasks in your cluster with it\. The following procedure creates a service with your task definition and places one task on your cluster\.

**To create a service from your task definition with the console**

1. On the **Task Definition: windows\-simple\-iis** registration confirmation page, choose **Actions**, **Create Service**\.

1. On the **Create Service** page, enter the following information and then choose **Create service**\.
   + **Cluster:** windows 
   + **Number of tasks:** 1
   + **Service name:** windows\-simple\-iis

**To create a service from your task definition with the AWS CLI**
+ Using the AWS CLI, run the following command to create your service\.

  ```
  aws ecs create-service --cluster windows --task-definition windows-simple-iis --desired-count 1 --service-name windows-simple-iis
  ```

## Step 6: View Your Service<a name="view_windows_service"></a>

After your service has launched a task into your cluster, you can view the service and open the IIS test page in a browser to verify that the container is running\.

**Note**  
It can take up to 15 minutes for your container instance to download and extract the Windows container base layers\.

**To view your service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, choose the **windows** cluster\.

1. In the **Services** tab, choose the **windows\-simple\-iis** service\.

1. On the **Service: windows\-simple\-iis** page, choose the task ID for the task in your service\.

1. On the **Task** page, expand the `iis` container to view its information\.

1. In the **Network bindings** of the container, you should see an **External Link** IP address and port combination link\. Choose that link to open the IIS test page in your browser\.  
![\[Windows simple IIS test page\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/windows-simple-iis.png)