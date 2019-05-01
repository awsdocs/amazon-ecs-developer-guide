# Managing Container Instances Remotely<a name="ec2-run-command"></a>

You can use the Amazon EC2 Run Command feature to securely and remotely manage the configuration of your Amazon ECS container instances\. Run Command provides a simple way of performing common administrative tasks without having to log on locally to the instance\. You can manage configuration changes across your clusters by simultaneously executing commands on multiple container instances\. Run Command reports the status and results of each command\.

Here are some examples of the types of tasks you can perform with Run Command:
+ Install or uninstall packages\.
+ Perform security updates\.
+ Clean up Docker images\.
+ Stop or start services\.
+ View system resources\.
+ View log files\.
+ Perform file operations\.

This topic covers basic installation of Run Command on the Linux variants of the Amazon ECS\-optimized AMI and a few simple use cases, but it is by no means exhaustive\. For more information about Run Command, see [Manage Amazon EC2 Instances Remotely](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/execute-remote-commands.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [Run Command IAM Policy](#run_command_iam_policy)
+ [Installing the SSM Agent on an Amazon ECS\-Optimized AMI](#install_ssm_agent)
+ [Using Run Command](#using_run_command)

## Run Command IAM Policy<a name="run_command_iam_policy"></a>

Before you can send commands to your container instances with Run Command, you must attach an IAM policy that allows access to the Amazon EC2 Systems Manager \(SSM\) APIs to the `ecsInstanceRole`\. The procedure below describes how to attach the `AmazonEC2RoleforSSM` managed policy to your container instance role so that instances launched with this role can use Run Command\.

**To attach the `AmazonEC2RoleforSSM` policy to your `ecsInstanceRole`**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Choose `ecsInstanceRole`\. If the role does not exist, follow the procedures in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) to create the role\.

1. Choose **Permissions**\.

1. In the **Managed Policies** section, choose **Attach Policy**\.

1. To narrow the available policies to attach, for **Filter**, type **AmazonEC2RoleforSSM**\.

1. Select the check box for the **AmazonEC2RoleforSSM** policy and choose **Attach Policy**\.

## Installing the SSM Agent on an Amazon ECS\-Optimized AMI<a name="install_ssm_agent"></a>

After you have attached the `AmazonEC2RoleforSSM` policy to your `ecsInstanceRole`, you can install the SSM agent on your container instances\. The SSM agent processes Run Command requests and configures the instances that are specified in the request\. Use the following procedures to install the SSM agent on your Amazon ECS\-optimized AMI container instances\.

**To manually install the SSM agent on existing Amazon ECS\-optimized AMI container instances**

1. [Connect to your container instance\.](instance-connect.md)

1. Install the SSM agent RPM\. The SSM agent is available in all Regions that Amazon ECS is available in\. Each Region has its own region\-specific download URL\. The example command below works for all Regions that Amazon ECS supports\. Avoid cross\-region data transfer costs for the RPM download by substituting the Region of your container instance\.

   ```
   [ec2-user ~]$ sudo yum install -y amazon-ssm-agent
   ```

**To install the SSM agent on new instance launches with Amazon EC2 user data**
+ Launch one or more container instances by following the procedure in [Launching an Amazon ECS Container Instance](launch_container_instance.md), but in [Step 7](launch_container_instance.md#instance-launch-user-data-step), copy and paste the user data script below into the **User data** field\. You can also add the commands from this user data script to another existing script that you may have to perform other tasks, such as setting the cluster name for the instance to register into\.

  ```
  #!/bin/bash
  # Install the SSM agent RPM
  yum install -y amazon-ssm-agent
  ```

## Using Run Command<a name="using_run_command"></a>

After you have attached the `AmazonEC2RoleforSSM` policy to your `ecsInstanceRole`, and installed the SSM agent on your container instances, you can start using Run Command to send commands to your container instances\. The following topic in the *Amazon EC2 User Guide for Linux Instances* explains how to run commands and shell scripts on your instances and view the resulting output:
+ [Running Shell Scripts with Run Command](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/remote-commands-shellcript.html)

For more information about Run Command, see [Manage Amazon EC2 Instances Remotely](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/execute-remote-commands.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Example: To update container instance software with Run Command**

A common use case for Run Command is to update the instance software on your entire fleet of container instances at one time\.

1. [Attach the `AmazonEC2RoleforSSM` policy to your `ecsInstanceRole`\.](#run_command_iam_policy)

1. Install the SSM agent on your container instances\. For more information, see [Installing the SSM Agent on an Amazon ECS\-Optimized AMI](#install_ssm_agent)\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation, choose **Commands**, **Run a command**\.

1. For **Command document**, choose **AWS\-RunShellScript**\.

1. In the **Target instances** section, choose **Select instances** and check the container instances to which to send the update command\.

1. In the **Commands** section, enter the command or commands to send to your container instances\. In this example, the command below updates the instance software:

   ```
   $ yum update -y
   ```

1. Choose **Run** to send the command to the specified instances\.

1. \(Optional\) Choose **View result**\.

1. \(Optional\) To view the command output, select a command from the list of recent commands\.  
![\[Run Command command list\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/command_list.png)

1. \(Optional\) Choose **Output**, **View Output**\. The image below shows a snippet of the container instance output for the yum update command\.
**Note**  
Unless you configure a command to save the output to an Amazon S3 bucket, then the command output is truncated at 2500 characters\.  
![\[Run Command command output\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/run-command-output.png)
