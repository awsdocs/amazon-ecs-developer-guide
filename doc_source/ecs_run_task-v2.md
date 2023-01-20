# Running a standalone task using the Amazon ECS console<a name="ecs_run_task-v2"></a>

We recommend that you deploy your application as a standalone task in some situations\. For example, suppose that you're developing an application but you're not ready to deploy it with the service scheduler\. If your application is a one\-time or periodic batch job, it doesn't make sense to keep running or restart when it finishes\.

To deploy your application to run continually or to place it behind a load balancer, create an Amazon ECS service\. For more information, see [Amazon ECS services](ecs_services.md)\.

To run a standalone task use one of the following procedures\.

Consider the following when you use the new console;
+ Task definitions that use the `awsvpc` network mode or services configured to use a load balancer must have a networking configuration\. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC\. 
+ For the EC2 launch type, the default task placement strategy is to distribute the tasks across Availability Zones and across container instances in the Availability Zone\.
+ For the **capacity provider strategy**, the console selects a compute option by default\. The following describes the order that the console uses to select a default:
  + If your cluster has a default capacity provider strategy defined, it is selected\.
  + If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy that uses the `FARGATE` capacity provider is selected\.
  + If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the **Use custom \(Advanced\)** option is selected and you need to manually define the strategy\.
  + If your cluster doesn't have a default capacity provider strategy defined and no capacity providers added to the cluster, the Fargate launch type is selected\.

**To run a task from the console**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster to run the standalone task in\.

   Determine the resource from where you launch the service\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_run_task-v2.html)

1. \(Optional\) Choose how your scheduled task is distributed across your cluster infrastructure\. Expand **Compute configuration**, and then do the following:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_run_task-v2.html)

1. For **Application type**, choose **Task**\.

1. For **Task definition**, choose the task definition family and revision to use\.
**Important**  
The console validates the selection to ensure that the selected task definition family and revision is compatible with the defined compute configuration\.

1. For **Desired tasks**, enter the number of tasks to launch\.

1. If your task definition uses the `awsvpc` network mode, expand **Networking**\. Use the following steps to specify a custom configuration\.

   1. For **VPC**, select the VPC to use\.

   1. For **Subnets**, select one or more subnets in the VPC that the task scheduler considers when placing your tasks\.
**Important**  
Only private subnets are supported for the `awsvpc` network mode\. Tasks do not receive public IP addresses\. Therefore, a NAT gateway is required for outbound internet access, and inbound internet traffic is routed through a load balancer\.

   1. For **Security group**, you can either select an existing security group or create a new one\. To use an existing security group, select the security group and move to the next step\. To create a new security group, choose **Create a new security group**\. You must specify a security group name, description, and then add one or more inbound rules for the security group\.

   1. For **Public IP**, choose whether to auto\-assign a public IP address to the elastic network interface \(ENI\) of the task\. AWS Fargate tasks can be assigned a public IP address when run in a public subnet so they have a route to the internet\. For more information, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

1. \(Optional\) To use a task placement strategy other than the default, expand **Task Placement**, and then choose from the following options\.

    For more information, see [Amazon ECS task placement](task-placement.md)\.
   + **AZ Balanced Spread** \- Distribute tasks across Availability Zones and across container instances in the Availability Zone\.
   + **AZ Balanced BinPack** \- Distribute tasks across Availability Zones and across container instances with the least available memory\.
   + **BinPack** \- Distribute tasks based on the least available amount of CPU or memory\.
   + **One Task Per Host** \- Place, at most, one task from the service on each container instance\.
   + **Custom** \- Define your own task placement strategy\. 

   If you chose **Custom**, define the algorithm for placing tasks and the rules that are considered during task placement\.
   + Under **Strategy**, for **Type** and **Field**, choose the algorithm and the entity to use for the algorithm\.

     You can enter a maximum of 5 strategies\.
   + Under **Constraint**, for **Type** and **Expression**, choose the rule and attribute to use for the constraint\.

     When you enter the **Expression**, do not enter the double quotation marks \(`" "`\)\. For example, to set the constraint to place tasks on T2 instances, for the **Expression**, enter **attribute:ecs\.instance\-type =\~ t2\.\***\.

     You can enter a maximum of 10 constraints\.

1. \(Optional\) To override the task IAM role, or task execution role that is defined in your task definition, expand **Task overrides**, and then complete the following steps:

   1. For **Task role**, choose an IAM role for this task to override the task IAM roles in the task definition\. For more information, see [Task IAM role](task-iam-roles.md)\.

      Only roles with the `ecs-tasks.amazonaws.com` trust relationship are displayed\. For instructions on how to create an IAM role for your tasks, see [Creating an IAM role and policy for your tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

   1. For **Task execution role**, choose a task execution role to override the task execution role specified in the task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

1. \(Optional\) To override the container commands and environment variables, expand **Container Overrides**, and then expand the container\.
   +  For **Command override**, enter the Docker command that is sent to the container instead of the command specified in the task definition\.

     For more information about the Docker run command, see [Docker Run reference](https://docs.docker.com/engine/reference/run/) in the Docker Reference Manual\.
   + To add an environment variable, choose **Add Environment Variable**\. For **Key**, enter the name of your environment variable\. For **Value**, enter a string value for your environment value \(without the surrounding double quotation marks \(`" "`\)\)\.

     AWS surrounds the strings with double quotation marks \(" "\) and passes the string to the container in the following format:

     ```
     MY_ENV_VAR="This variable contains a string."
     ```

1. \(Optional\) To help identify your task, expand the **Tags** section, and then configure your tags\.

   To have Amazon ECS automatically tag all newly launched tasks with the cluster name and the task definition tags, select **Turn on Amazon ECS managed tags**, and then select **Task definitions**\.

   Add or remove a tag\.
   + \[Add a tag\] Choose **Add tag**, and then do the following:
     + For **Key**, enter the key name\.
     + For **Value**, enter the key value\.
   + \[Remove a tag\] Next to the tag, choose **Remove tag**\.

1. Choose **Deploy**\.