# Manage container instances remotely using AWS Systems Manager<a name="ec2-run-command"></a>

You can use the Run Command capability in AWS Systems Manager \(Systems Manager\) to securely and remotely manage the configuration of your Amazon ECS container instances\. Run Command provides a simple way to perform common administrative tasks without logging on locally to the instance\. You can manage configuration changes across your clusters by simultaneously executing commands on multiple container instances\. Run Command reports the status and results of each command\.

Here are some examples of the types of tasks you can perform with Run Command:
+ Install or uninstall packages\.
+ Perform security updates\.
+ Clean up Docker images\.
+ Stop or start services\.
+ View system resources\.
+ View log files\.
+ Perform file operations\.

For more information about Run Command, see [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [Run Command IAM policy](#run_command_iam_policy)
+ [Using Run Command](#using_run_command)

## Run Command IAM policy<a name="run_command_iam_policy"></a>

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

## Using Run Command<a name="using_run_command"></a>

After you attach Systems Manager managed policies to your `ecsInstanceRole` and verify that AWS Systems Manager Agent \(SSM Agent\) is installed on your container instances, you can start using Run Command to send commands to your container instances\. For information about running commands and shell scripts on your instances and viewing the resulting output, see [Running Commands Using Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html) and [Run Command Walkthroughs](https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command-walkthroughs.html) in the *AWS Systems Manager User Guide\.* 

**Example: To update container instance software with Run Command**

A common use case for Run Command is to update the instance software on your entire fleet of container instances at one time\.

1. [Attach Systems Manager managed policies to your `ecsInstanceRole`\.](#run_command_iam_policy)

1. Verify that SSM Agent is installed on your container instances\. For more information, see [Manually install SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-manual-agent-install.html)\.

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