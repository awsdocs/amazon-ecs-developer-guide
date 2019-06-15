# Manage Container Instances Remotely Using AWS Systems Manager<a name="ec2-run-command"></a>

You can use the Run Command capability in AWS Systems Manager to securely and remotely manage the configuration of your Amazon ECS container instances\. Run Command provides a simple way to perform common administrative tasks without logging on locally to the instance\. You can manage configuration changes across your clusters by simultaneously executing commands on multiple container instances\. Run Command reports the status and results of each command\.

Here are some examples of the types of tasks you can perform with Run Command:
+ Install or uninstall packages\.
+ Perform security updates\.
+ Clean up Docker images\.
+ Stop or start services\.
+ View system resources\.
+ View log files\.
+ Perform file operations\.

This topic covers basic installation of Run Command on the Linux variants of the Amazon ECS\-optimized AMI and a few simple use cases, but it is not comprehensive\. For more information about Run Command, see [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [Run Command IAM Policy](#run_command_iam_policy)
+ [Installing SSM Agent on an Amazon ECS\-Optimized AMI](#install_ssm_agent)
+ [Using Run Command](#using_run_command)

## Run Command IAM Policy<a name="run_command_iam_policy"></a>

Before you can send commands to your container instances with Run Command, you must attach an IAM policy that allows `ecsInstanceRole` to have access to the Systems Manager APIs\. The following procedure describes how to attach the Systems Manager managed policies to your container instance role so that instances launched with this role can use Run Command\.

**To attach the Systems Manager policies to your `ecsInstanceRole`**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Choose `ecsInstanceRole`\. If the role does not exist, follow the procedures in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) to create the role\.

1. Choose the **Permissions** tab\.

1. Choose **Attach policies**\.

1. To narrow the available policies to attach, for **Filter**, type `SSM`\.

1. In the list of policies, select the box next **AmazonSSMManagedInstanceCore**\. This policy enables you to provide the minimum permissions that are necessary to use Systems Manager\.

   For information about other policies you can provide for Systems Manager operations, see [Create an IAM Instance Profile for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-profile.html) in the *AWS Systems Manager User Guide*\.

1. Choose **Attach Policy**\.

## Installing SSM Agent on an Amazon ECS\-Optimized AMI<a name="install_ssm_agent"></a>

After you attach Systems Manager policies to your `ecsInstanceRole`, you can install AWS Systems Manager Agent \(SSM Agent\) on your container instances\. SSM Agent is Amazon software that can be installed and configured on an Amazon EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. SSM Agent processes Run Command requests and configures the instances that are specified in the request\. Use the following procedures to install SSM Agent on your Amazon ECS\-optimized AMI container instances\.

**Note**  
SSM Agent is available in all Regions that Systems Manager is available in\. \(For a list of supported Regions, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.\) Each Region has its own region\-specific download URL, but the following commands use your current specified AWS Region\. For more information about SSM Agent, see [Working with SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) in the *AWS Systems Manager User Guide*\.

**To manually install SSM Agent on existing Amazon ECS\-optimized AMI container instances**

1. [Connect to your container instance\.](instance-connect.md)

1. Run the following command to install the SSM Agent RPM\. 

   ```
   [ec2-user ~]$ sudo yum install -y amazon-ssm-agent
   ```

**To install SSM Agent on new instance launches with Amazon EC2 user data**
+ Launch one or more container instances by following the procedure in [Launching an Amazon ECS Container Instance](launch_container_instance.md), but in [Step 7](launch_container_instance.md#instance-launch-user-data-step), copy and paste the user data script below into the **User data** field\. You can also add the commands from this user data script to another existing script that you may have to perform other tasks, such as setting the cluster name for the instance to register into\.

  ```
  #!/bin/bash
  # Install the SSM Agent RPM
  yum install -y amazon-ssm-agent
  ```

## Using Run Command<a name="using_run_command"></a>

After you attach Systems Manager managed policies to your `ecsInstanceRole`, and install SSM Agent on your container instances, you can start using Run Command to send commands to your container instances\. For information about running commands and shell scripts on your instances and viewing the resulting output, see [Running Commands Using Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html) and [Run Command Walkthroughs](https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command-walkthroughs.html) in the *AWS Systems Manager User Guide\.* 

**Example: To update container instance software with Run Command**

A common use case for Run Command is to update the instance software on your entire fleet of container instances at one time\.

1. [Attach Systems Manager managed policies to your `ecsInstanceRole`\.](#run_command_iam_policy)

1. Install SSM Agent on your container instances\. For more information, see [Installing SSM Agent on an Amazon ECS\-Optimized AMI](#install_ssm_agent)\.

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager](https://console.aws.amazon.com/systems-manager)\.

1. In the left navigation pane, choose **Run Command**, and then choose **Run command**\.

1. For **Command document**, choose **AWS\-RunShellScript**\.

1. In the **Commands** section, enter the command or commands to send to your container instances\. In this example, the following command updates the instance software:

   ```
   $ yum update -y
   ```

1. In the **Target instances** section, select the boxes next to the container instances where you want to run the update command\.

1. Choose **Run** to send the command to the specified instances\.

1. \(Optional\) Choose the refresh icon to monitor the command status\.

1. \(Optional\) In **Targets and output**, choose the button next to the instance ID, and then choose **View output**\.