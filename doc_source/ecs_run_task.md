# Run a standalone task in the classic Amazon ECS console<a name="ecs_run_task"></a>

We recommend that you deploy your application as a standalone task in some situations\. For example, suppose that you're developing an application but you're not ready to deploy it with the service scheduler\. If your application is a one\-time or periodic batch job, it doesn't make sense to keep running or restart when it finishes\.

To deploy your application to run continually or to place it behind a load balancer, create an Amazon ECS service\. For more information, see [Amazon ECS services](ecs_services.md)\.

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
   + **Custom** \- Define your own task placement strategy\. 

    For more information, see [Amazon ECS task placement](task-placement.md)\.

1. \(Optional\) To send command, environment variable, task IAM role, or task execution role overrides to one or more containers in your task definition, choose **Advanced Options** and complete the following steps:
**Note**  
If you intend to use the parameter values from your task definition, you don't need to specify overrides\. These fields are only used to override the values that are specified in the task definition\.

   1. For **Task Role Override**, choose an IAM role for this task to override the task IAM role that's specified in the task definition\. For more information, see [IAM roles for tasks](task-iam-roles.md)\.

      Only roles with the `ecs-tasks.amazonaws.com` trust relationship are displayed\. For instructions on how to create an IAM role for your tasks, see [Creating an IAM role and policy for your tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

   1. For **Task Execution Role Override**, choose a task execution role to override the task execution role specified in the task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

1. \(Optional\) To override the container commands and environment variables, expand **Container Overrides**, and then expand the container\.
   +  For **Command override**, enter the Docker command that is sent to the container instead of the command specified in the task definition\.

     For more information about the Docker run command, see [Docker Run reference](https://docs.docker.com/engine/reference/run/) in the Docker Reference Manual\.
   + To add an environment variable, choose **Add Environment Variable**\. For **Key**, enter the name of your environment variable\. For **Value**, enter a string value for your environment value \(without the surrounding double quotation marks \(`" "`\)\)\.

     AWS surrounds the strings with double quotation marks \(" "\) and passes the string to the container in the following format:

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
#### [ Command line ]

Use the `run-task` command\. For more information, see [run\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/run-task.html) in the *AWS Command Line Interface Reference*\. 

------