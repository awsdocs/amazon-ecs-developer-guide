# Run a standalone task using the new console<a name="ecs_run_task-v2"></a>

We recommend that you deploy your application as a standalone task in some situations\. For example, suppose that you're developing an application but you're not ready to deploy it with the service scheduler\. If your application is a one\-time or periodic batch job, it doesn't make sense to keep running or restart when it finishes\.

To deploy your application to run continually or to place it behind a load balancer, create an Amazon ECS service\. For more information, see [Amazon ECS services](ecs_services.md)\.

To run a standalone task use one of the following procedures\.

If you are creating a Windows service for the Fargate launch type, you must use the classic console\. 

------
#### [ New console from the Cluster page ]

**To run a standalone task \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

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
#### [ New Console from the task definition page ]

**To run a standalone task \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Task definitions** page, choose the task definition\.

1. Choose **Deploy**, **Run task**\.

1. From **Existing cluster**, choose the cluster where the task runs\.

1. The **Compute configuration** section can be expanded to change the compute option for your service to use\. By default, the console will select a compute option for you so in most cases you can go to the next step\. The following describes the order that the console uses to select a default:
   + If your cluster has a default capacity provider strategy defined, it will be selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy using the `FARGATE` capacity provider will be selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the **Use custom \(Advanced\)** option is selected and you will need to manually define the strategy\.
   + If your cluster doesn't have a default capacity provider strategy defined and no capacity providers added to the cluster, the Fargate launch type is selected\.

1. For **Desired tasks**, specify the number of tasks to launch and maintain in the service\.

1. The **Networking** section can be expanded to define the network configuration for the service\. Task definitions that use the `awsvpc` network mode or services configured to use a load balancer must have a networking configuration\. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC\. Use the following steps to specify a custom configuration\.

   1. For **VPC**, select the VPC to use\.

   1. For **Subnets**, select one or more subnets in the VPC that the task scheduler should consider when placing your tasks\.

   1. For **Security group**, you can either select an existing security group or create a new one\. To use an existing security group, select the security group and move to the next step\. To create a new security group, choose **Create a new security group**\. You must specify a security group name, description, and then add one or more inbound rules for the security group\.

   1. For **Public IP**, choose whether to auto\-assign a public IP address to the elastic network interface \(ENI\) of the task\. Tasks that are launched on AWS Fargate can be assigned a public IP address when run using a public subnet so they have a route to the internet\. For more information, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

1. \(Optional\) The **Tags** section can be expanded to add tags, in the form of key\-value pairs, to the task\.

------