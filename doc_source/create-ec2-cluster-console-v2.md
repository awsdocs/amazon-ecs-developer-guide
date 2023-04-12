# Creating a cluster for the Amazon EC2 launch type using the console<a name="create-ec2-cluster-console-v2"></a>

You can create an Amazon ECS cluster using the new Amazon ECS experience\. Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) and assign the appropriate IAM permission\. For more information, see [Cluster examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies)\. The new Amazon ECS experience provides a simple way to create the resources that are needed by an Amazon ECS cluster by creating a AWS CloudFormation stack\. 

To make the cluster creation process as easy as possible, the console has default selections for many choices which we describe below\. There are also help panels available for most of the sections in the console which provide further context\. 

You can register Amazon EC2 instances when you create the cluster or register additional instances with the cluster after it has been created\.

You can modify the following default options:
+ Change the subnets where tasks and services launch into by default\.
+ Change the default namespace associated with the cluster\. A namespace allows services that you create in the cluster can connect to the other services in the namespace without additional configuration\. The default namespace is the same as the cluster name\. 

  For more information, see [Interconnecting services](interconnecting-services.md)\.
+ Turn on Container Insights\.

  CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. Container Insights also provides diagnostic information, such as container restart failures, that you use to isolate issues and resolve them quickly\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Add tags to help you identify your clusters\.

## Auto Scaling group options<a name="capacity-providers"></a>

If you want to use Spot Instances in your Auto Scaling group, you must use the classic console to create the cluster\. For more information, see [Creating a cluster using the classic console](create_cluster.md)\.

When you use Amazon EC2 instances, you must specify an Auto Scaling group to manage the infrastructure that your tasks and services run on\. 

When you choose to create a new Auto Scaling group, it is automatically configured for the following behavior:
+ Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.
+ Amazon ECS will not prevent Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.

You configure the following Auto Scaling group properties which determine the type and number of instances to launch for the group:
+ The Amazon ECS\-optimized AMI\. 
+ The instance type\.
+ The SSH key pair that proves your identity when you connect to the instance\. For information about how to create SSH keys, see [Amazon EC2 key pairs and Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ The minimum number of instances to launch for the Auto Scaling group\. 
+ The maximum number of instances that are started for the Auto Scaling group\. 

  In order for the group to scale out, the maximum must be greater than 0\.

Amazon ECS creates an Amazon EC2 Auto Scaling launch template and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. The values that you specified for the AMI, the instance types, and the SSH key pair are part of the launch template\. The templates are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. The Auto Scaling groups are prefixed with `<ClusterName>-ECS-Infra-ECSAutoScalingGroup`\.

Instances launched for the Auto Scaling group use the launch template\.

**To create a new cluster \(Amazon ECS console\)**

Before you begin, assign the appropriate IAM permission\. For more information, see [Cluster examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies)\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter a unique name\.

   The name can contain up to 255 letters \(uppercase and lowercase\), numbers, and hyphens\.

1. \(Optional\) To change the name of the default namespace\. for **Namespace**, enter a unique name\.

1. \(Optional\) To change the VPC and subnets where your tasks and services launch, under **Networking**, perform any of the following operations:
   + To remove a subnet, under **Subnets**, choose **X** for each subnet that you want to remove\.
   + To change to a VPC other than the **default** VPC, under **VPC**, choose an existing **VPC**, and then under **Subnets**, select each subnet\.

1. \(Optional\) To add Amazon EC2 instances to your cluster, expand **Infrastructure**, and then select **Amazon EC2 instances**\. Next, configure the Auto Scaling group which acts as the capacity provider:

   1. To using an existing Auto Scaling group, from **Auto Scaling group \(ASG\)**, select the group\.

   1. To create a Auto Scaling group, from **Auto Scaling group \(ASG\)**, select **Create new group**, and then provide the following details about the group:
      + For **Operating system/Architecture**, choose the Amazon ECS\-optimized AMI for the Auto Scaling group instances\.
      + For **EC2 instance type**, choose the instance type for your workloads\.

         Managed scaling works best if your Auto Scaling group uses the same or similar instance types\. 
      + For **SSH key pair**, choose the pair that proves your identity when you connect to the instance\.
      + For **Capacity**, enter the minimum number and the maximum number of instances to launch in the Auto Scaling group\. 

1. \(Optional\) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

1. \(Optional\) To manage the cluster tags, expand **Tags**, and then perform one of the following operations:

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Create**\.