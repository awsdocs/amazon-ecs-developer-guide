# Creating a service using the new console<a name="create-service-console-v2"></a>

**Important**  
If you are creating a Windows service for the Fargate launch type, you must use the classic console\. For more information, see [Creating a service using the classic console](create-service-console-v1.md)\.

You can create an Amazon ECS service using the new Amazon ECS console\. To make the service creation process as easy as possible, the console has default selections for many choices which we describe below\. There are also help panels available for most of the sections in the console which provide further context\.

**To create a service using the new console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster to create the service in\.

1. From the **Services** tab, choose **Deploy**\.

1. The **Compute configuration** section can be expanded to change the compute option for your service to use\. By default, the console will select a compute option for you so in most cases you can go to the next step\. The following describes the order that the console uses to select a default:
   + If your cluster has a default capacity provider strategy defined, it will be selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy using the `FARGATE` capacity provider will be selected\.
   + If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the **Use custom \(Advanced\)** option is selected and you will need to manually define the strategy\.
   + If your cluster doesn't have a default capacity provider strategy defined and no capacity providers added to the cluster, the Fargate launch type is selected\.

1. For **Application type**, select **Service**\.

1. For **Task definition**, choose the task definition family and revision to use\.
**Important**  
The console validates that the selected task definition family and revision is compatible with the defined compute configuration\. If you receive a warning, verify both your task definition compatibility and the compute configuration selected\.

1. For **Service name**, specify a name for your service\.

1. For **Desired tasks**, specify the number of tasks to launch and maintain in the service\.

1. The **Deployment options** section can be expanded to change the minimum healthy percent and maximum percent of running tasks allowed during a service deployment\. The console has default values for the most common use case selected\.
**Note**  
Currently, only the **Rolling update** \(`ECS`\) deployment type is supported\. To use any other deployment type, switch to the old console\.

1. \(Optional\) The **Load balancing** section can be expanded to configure a load balancer for your service\. Use the following steps to configure your service to use an Application Load Balancer\.

   1. For **Load balancer type**, select **Application Load Balancer**\.

   1. Choose **Create a new load balancer** to create a new Application Load Balancer or **Use an existing load balancer** to select an existing Application Load Balancer\.

   1. When creating a new load balancer, for **Load balancer name**, specify a unique name for your load balancer\. When using an existing load balancer, for **Load balancer**, select your existing load balancer\.

   1. For **Listener**, specify a port and protocol for the Application Load Balancer to listen for connection requests on\. By default, the load balancer will be configured to use port 80 and HTTP\.

   1. For **Target group name**, specify a name and a protocol for the target group that the Application Load Balancer will route requests to\. By default, the target group will route requests to the first container defined in your task definition\.

   1. For **Health check path**, specify a path that exists within your container where the Application Load Balancer should periodically send requests to verify the connection health between the Application Load Balancer and the container\. By default, a path of `/` is used which is the root directory\.

   1. For **Health check grace period**, specify the amount of time \(in seconds\) that the service scheduler should ignore unhealthy Elastic Load Balancing target health checks for\.

1. The **Networking** section can be expanded to define the network configuration for the service\. Task definitions that use the `awsvpc` network mode or services configured to use a load balancer must have a networking configuration\. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC\. Use the following steps to specify a custom configuration\.

   1. For **VPC**, select the VPC to use\.

   1. For **Subnets**, select one or more subnets in the VPC that the task scheduler should consider when placing your tasks\.

   1. For **Security group**, you can either select an existing security group or create a new one\. To use an existing security group, select the security group and move to the next step\. To create a new security group, choose **Create a new security group**\. You must specify a security group name, description, and then add one or more inbound rules for the security group\.

   1. For **Public IP**, choose whether to auto\-assign a public IP address to the elastic network interface \(ENI\) of the task\. Tasks that are launched on AWS Fargate can be assigned a public IP address when run using a public subnet so they have a route to the internet\. For more information, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

1. \(Optional\) The **Tags** section can be expanded to add tags, in the form of key\-value pairs, to the service\.