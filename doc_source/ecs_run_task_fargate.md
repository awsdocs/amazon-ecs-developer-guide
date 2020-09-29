# Running a task using the Fargate launch type<a name="ecs_run_task_fargate"></a>

To run a task using the Fargate launch type, do the following:

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions** and select the task definition to run\.
   + To run the latest revision of a task definition shown here, select the box to the left of the task definition to run\.
   + To run an earlier revision of a task definition shown here, select the task definition to view all active revisions, then select the revision to run\.

1. Choose **Actions**, **Run Task**\.

1. On the **Run Task** page, complete the following steps\.

   1. Choose either a capacity provider strategy or a launch type\.
      + To use a **Capacity provider strategy**, choose **Switch to capacity provider strategy** and then choose whether your task should use the default capacity provider strategy defined for the cluster or a custom capacity provider strategy\. A capacity provider must already be associated with the cluster in order to be used in a custom capacity provider strategy\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.
      + To use a **Launch type**, choose **Switch to launch type** and select **FARGATE**\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.

   1. For **Platform version**, choose the platform version to run your task on\. For more information about platform versions, see [AWS Fargate platform versions](platform_versions.md)\.

   1. For **Cluster**, choose the cluster to use\. 

   1. For **Number of tasks**, type the number of tasks to launch with this task definition\.

   1. For **Task group**, type the name of the task group\.

1. In the **VPC and security groups** section, complete the following steps:

   1. For **Cluster VPC**, choose the VPC for your tasks to use\. Ensure that the VPC that you choose is not configured to require dedicated hardware tenancy, as that is not supported by Fargate tasks\.

   1. For **Subnets**, choose the available subnets for your task\.

   1. For **Security groups**, a security group has been created for your task that allows HTTP traffic from the internet \(0\.0\.0\.0/0\)\. To edit the name or the rules of this security group, or to choose an existing security group, choose **Edit** and then modify your security group settings\.

   1. For **Auto\-assign public IP**, choose **ENABLED** if you want the elastic network interface attached to the Fargate task to be assigned a public IP address\. This is required if your task needs outbound network access, for example to pull an image\. If outbound network access is not required, then you can choose **DISABLED**\.

1. In the **Advanced Options** section, complete the following steps:

   1. \(Optional\) To send command, environment variable, task IAM role, or task execution role overrides to one or more containers in your task definition, choose **Advanced Options** and complete the following steps:
**Note**  
If you will be using the parameter values from your task definition there is no need to specify overrides\. These fields are only used to override the values specified in the task definition\.

     1. For **Task Role Override**, choose an IAM role for this task to override the task IAM role specified in the task definition\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

        Only roles with the `ecs-tasks.amazonaws.com` trust relationship are shown here\. For more information about creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

     1. For **Task Execution Role Override**, choose a task execution role to override the task execution role specified in the task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

     1. For **Container Overrides**, choose a container to which to send a command or environment variable override\.
        + **For a command override:** For **Command override**, type the command override to send\. If your container definition does not specify an `ENTRYPOINT`, the format should be a comma\-separated list of non\-quoted strings\. For example:

          ```
          /bin/sh,-c,echo,$DATE
          ```

          If your container definition does specify an `ENTRYPOINT` \(such as sh,\-c\), the format should be an unquoted string, which is surrounded with double quotes and passed as an argument to the `ENTRYPOINT` command\. For example:

          ```
          while true; do echo $DATE > /var/www/html/index.html; sleep 1; done
          ```
        + **For environment variable overrides:** Choose **Add Environment Variable**\. For **Key**, type the name of your environment variable\. For **Value**, type a string value for your environment value \(without surrounding quotes\)\.  
![\[Environment variable override\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/env_var.png)

          This environment variable override is sent to the container as:

          ```
          MY_ENV_VAR="This variable contains a string."
          ```

1. In the **Task tagging configuration** section, complete the following steps:

   1. Select **Enable ECS managed tags** if you want Amazon ECS to automatically tag each task with the Amazon ECS managed tags\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

   1. For **Propagate tags from**, select one of the following:
      + **Do not propagate** – This option will not propagate any tags\.
      + **Task Definitions** – This option will propagate the tags specified in the task definition to the task\.
**Note**  
If you specify a tag with the same `key` in the **Tags** section, it will override the tag propagated from the task definition\.

1. In the **Tags** section, specify the key and value for each tag to associate with the task\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. Review your task information and choose **Run Task**\.
**Note**  
If your task moves from `PENDING` to `STOPPED`, or if it displays a `PENDING` status and then disappears from the listed tasks, your task may be stopping due to an error\. For more information, see [Checking stopped tasks for errors](stopped-task-errors.md) in the troubleshooting section\.