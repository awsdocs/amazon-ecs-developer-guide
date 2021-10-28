# Run a standalone task<a name="ecs_run_task"></a>

We recommend that you deploy your application as a standalone task in some situations\. For example, suppose that you're developing an application but you're not ready to deploy it with the service scheduler\. If your application is a one\-time or periodic batch job, it doesn't make sense to keep running or restart when it finishes\.

To deploy your application to run continually or to place it behind a load balancer, create an Amazon ECS service\. For more information, see [Amazon ECS services](ecs_services.md)\.

To run a standalone task use one of the following procedures\.

If you are creating a Windows service for the Fargate launch type, you must use the classic console\. 

------
#### [ New console ]

To run a standalone task using the new console

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster to run the standalone task in\.

1. From the **Tasks** tab, choose **Run new task**\.

1. The **Compute configuration** section can be expanded to change the compute option for your service to use\. By default, the console selects a compute option for you\. So, in most cases, you can go to the next step\. The following describes the order that the console uses to select a default:
   + If your cluster has a default capacity provider strategy defined, that capacity provider strategy is selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy that uses the `FARGATE` capacity provider is selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the **Use custom \(Advanced\)** option is selected\. For this, you must manually define the strategy\.
   + If your cluster doesn't have a default capacity provider strategy defined and no capacity providers are added to the cluster, the Fargate launch type is selected\.

1. For **Application type**, choose **Task**\.

1. For **Task definition**, choose the task definition family and revision to use\.
**Important**  
The console validates the selection to ensure that the selected task definition family and revision is compatible with the defined compute configuration\.

1. For **Desired tasks**, specify the number of tasks to run in the cluster\.

1. The **Networking** section can be expanded to define the Amazon VPC, subnet, and security group configuration if your task requires it\. Task definitions that use the `awsvpc` network mode must have a networking configuration\. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC\. If your application requires it, create a new security group\.

1. \(Optional\) The **Tags** section can be expanded to add tags, as key\-value pairs, to the service\.

1. Choose **Deploy**\.

------
#### [ Classic console ]

To run a standalone task using the classic console

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions** and select the task definition to run\.
   + To run the latest revision of a task definition, select the box to the left of the task definition to run\.
   + To run an earlier revision of a task definition, select the task definition to view all active revisions\. Last, select the revision to run\.

1. Choose **Actions**, **Run Task**\.

1. On the **Run Task** page, complete the following steps\.

   1. Choose either a capacity provider strategy or a launch type\.
      + To use a **Capacity provider strategy** and choose **Switch to capacity provider strategy**\. Then, choose whether your task uses the default capacity provider strategy that's defined for the cluster or a custom capacity provider strategy\. A capacity provider must be associated with the cluster to be used in a custom capacity provider strategy\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.
      + To use a **Launch type**, choose **Switch to launch type** and select either **EC2** or **EXTERNAL**\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.

   1. For **Cluster**, choose the cluster to use\.

   1. For **Number of tasks**, enter the number of tasks to launch with this task definition\.

   1. For **Task group**, enter the name of the task group\.

1. If your task definition uses the `awsvpc` network mode, complete these substeps\. Otherwise, proceed to the next step\.

   1. For **Cluster VPC**, choose the VPC that your container instances reside in\.

   1. For **Subnets**, choose the available subnets for your task\.
**Important**  
Only private subnets are supported for the `awsvpc` network mode\. Tasks don't receive public IP addresses\. Therefore, a NAT gateway is required for outbound internet access, and inbound internet traffic is routed through a load balancer\.

   1. For **Security groups**, a security group was created for your task that allows HTTP traffic from the internet \(0\.0\.0\.0/0\)\. To edit the name or the rules of this security group, choose **Edit** and then modify your security group settings\. Do the same if you want to choose an existing security group\.

1. \(Optional\) For **Task Placement**, you can specify how tasks are placed using task placement strategies and constraints\. Choose from the following options:
   + **AZ Balanced Spread** \- Distribute tasks across Availability Zones and across container instances in the Availability Zone\.
   + **AZ Balanced BinPack** \- Distribute tasks across Availability Zones and across container instances with the least available memory\.
   + **BinPack** \- Distribute tasks based on the least available amount of CPU or memory\.
   + **One Task Per Host** \- Place, at most, one task from the service on each container instance\.
   + **Custom** \- Define your own task placement strategy\. For examples, see [Amazon ECS task placement](task-placement.md)\.

    For more information, see [Amazon ECS task placement](task-placement.md)\.

1. \(Optional\) To send command, environment variable, task IAM role, or task execution role overrides to one or more containers in your task definition, choose **Advanced Options** and complete the following steps:
**Note**  
If you intend to use the parameter values from your task definition, you don't need to specify overrides\. These fields are only used to override the values that are specified in the task definition\.

   1. For **Task Role Override**, choose an IAM role for this task to override the task IAM role that's specified in the task definition\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

      Only roles with the `ecs-tasks.amazonaws.com` trust relationship are displayed\. For instructions on how to create an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

   1. For **Task Execution Role Override**, choose a task execution role to override the task execution role specified in the task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

   1. For **Container Overrides**, choose a container to which to send a command or environment variable override\.
      + **For a command override:** For **Command override**, enter the command override to send\. If your container definition doesn't specify an `ENTRYPOINT`, the format is a comma\-separated list of non\-quoted strings\.

        ```
        /bin/sh,-c,echo,$DATE
        ```

        If your container definition specifies an `ENTRYPOINT` \(such as sh,\-c\), the format is an unquoted string\. This string is surrounded with double quotation marks \(" "\) and passed as an argument to the `ENTRYPOINT` command\.

        ```
        while true; do echo $DATE > /var/www/html/index.html; sleep 1; done
        ```
      + **For environment variable overrides:** Choose **Add Environment Variable**\. For **Key**, enter the name of your environment variable\. For **Value**, enter a string value for your environment value \(without the surrounding double quotation marks \(`" "`\)\)\.  
![\[Environment variable override\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/env_var.png)

        This environment variable override is sent to the container in the following format:

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
If your task moves from the `PENDING` to the `STOPPED` status, your task might be stopping because of an error\. This is also the case if it displays a `PENDING` status and then disappears from the listed tasks\. For more information, see [Checking stopped tasks for errors](stopped-task-errors.md) in the troubleshooting section\.

------