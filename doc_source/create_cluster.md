# Creating a cluster<a name="create_cluster"></a>

You can create an Amazon ECS cluster using the AWS Management Console, as described in this topic\. Before you begin, be sure that you've completed the steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\. You can register Amazon EC2 instances during cluster creation or register additional instances with the cluster after creating it\.

The console cluster creation wizard provides a simple way to create the resources that are needed by an Amazon ECS cluster by creating a AWS CloudFormation stack\. It also lets you customize several common cluster configuration options\. However, the wizard does not allow you to customize every resource option\. For example, you can't use the wizard to customize the container instance AMI ID\. If your requirements extend beyond what is supported in this wizard, consider using our reference architecture at [https://github\.com/awslabs/ecs\-refarch\-cloudformation](https://github.com/awslabs/ecs-refarch-cloudformation)\.

If you add or modify the underlying cluster resources directly after they are created by the wizard you may receive an error when attempting to delete the cluster\. AWS CloudFormation refers to this as *stack drift*\. For more information on detecting drift on an existing AWS CloudFormation stack, see [Detect Drift on an Entire CloudFormation Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html) in the *AWS CloudFormation User Guide*\.

**To create a cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. For **Select cluster compatibility**, choose one of the following options and then choose **Next Step**:
   + **Networking only**– With this option, you can launch a cluster with a new VPC to use for Fargate tasks\. The `FARGATE` and `FARGATE_SPOT` capacity providers will be automatically associated with the cluster\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.

     You can run tasks using the Fargate launch type\. The Fargate launch type allows you to run your containerized applications without the need to provision and manage the backend infrastructure\. When you run a task with a Fargate\-compatible task definition, Fargate launches the containers for you\.
   + **EC2 Linux \+ Networking**– With this option you can launch a cluster of tasks using the EC2 launch type using Linux containers\. The EC2 launch type allows you to run your containerized applications on a cluster of Amazon EC2 instances that you manage\.
   + **EC2 Windows \+ Networking** – With this option you can launch a cluster of tasks using the EC2 launch type using Windows containers\. The EC2 launch type allows you to run your containerized applications on a cluster of Amazon EC2 instances that you manage\. For more information, see [Windows containers](ECS_Windows.md)\.

## Using the Networking only template<a name="create-cluster-fargate"></a>

If you chose the **Networking only** cluster template, complete the following steps\. Otherwise, skip to [Using the EC2 Linux \+ Networking or EC2 Windows \+ Networking template](#create-cluster-ec2)\.

**Using the **Networking only** cluster template**

1. On the **Configure cluster** page, enter a **Cluster name**\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. In the **Networking** section, configure the VPC for your cluster\. You can keep the default settings, or you can modify these settings with the following steps\.

   1. \(Optional\) If you choose to create a new VPC, for **CIDR Block**, select a CIDR block for your VPC\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

   1. For **Subnets**, select the subnets to use for your VPC\. You can keep the default settings, or you can modify them to meet your needs\.

1. In the **Tags** section, specify the key and value for each tag to associate with the cluster\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. In the **CloudWatch Container Insights** section, choose whether to enable Container Insights for the cluster\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.

1. Choose **Create**\.

## Using the EC2 Linux \+ Networking or EC2 Windows \+ Networking template<a name="create-cluster-ec2"></a>

If you chose the **EC2 Linux \+ Networking** or **EC2 Windows \+ Networking** templates, complete the following steps\.

**Using the **EC2 Linux \+ Networking** or **EC2 Windows \+ Networking** cluster template**

1. For **Cluster name**, enter a name for your cluster\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. \(Optional\) To create a cluster with no resources, choose **Create an empty cluster**, **Create**\.

1. For **Provisioning model**, choose one of the following instance types:
   + **On\-Demand Instance**– With On\-Demand Instances, you pay for compute capacity by the hour with no long\-term commitments or upfront payments\.
   + **Spot**– Spot Instances allow you to bid on spare Amazon EC2 computing capacity for up to 90% off the On\-Demand price\. For more information, see [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)\.
**Note**  
Spot Instances are subject to possible interruptions\. We recommend that you avoid Spot Instances for applications that can't be interrupted\. For more information, see [Spot Instance Interruptions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html)\.

1. For Spot Instances, do the following; otherwise, skip to the next step\.

   1. For **Spot Instance allocation strategy**, choose the strategy that meets your needs\. For more information, see [Spot Fleet Allocation Strategy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html#spot-fleet-allocation-strategy)\.

   1. For **Maximum bid price \(per instance/hour\)**, specify a bid price\. If your bid price is lower than the Spot price for the instance types that you selected, your Spot Instances are not launched\.

1. For **EC2 instance type**, choose the Amazon EC2 instance type for your container instances\. The instance type that you select determines the EC2 AMI Ids and resources available for your tasks\. For GPU workloads, choose an instance type from the P2 or P3 instance family\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.

1. For **Number of instances**, choose the number of EC2 instances to launch into your cluster\. These instances are launched using the latest Amazon ECS\-optimized Amazon Linux AMI required by the instance type you chose\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

1. For **EC2 AMI Id**, choose the Amazon ECS\-optimized AMI for your container instances\. The available AMIs will be determined by the Region and EC2 instance type you chose\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

1. For **EBS storage \(GiB\)**, choose the size of the Amazon EBS volume to use for data storage on your container instances\. You can increase the size of the data volume to allow for greater image and container storage\.

1. For **Key pair**, choose an Amazon EC2 key pair to use with your container instances for SSH access\. If you do not specify a key pair, you cannot connect to your container instances with SSH\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. In the **Networking** section, configure the VPC to launch your container instances into\. By default, the cluster creation wizard creates a new VPC with two subnets in different Availability Zones, and a security group open to the internet on port 80\. This is a basic setup that works well for an HTTP service\. However, you can modify these settings by following the substeps below\.

   1. For **VPC**, create a new VPC, or select an existing VPC\.

   1. \(Optional\) If you chose to create a new VPC, for **CIDR Block**, select a CIDR block for your VPC\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

   1. For **Subnets**, select the subnets to use for your VPC\. If you chose to create a new VPC, you can keep the default settings or you can modify them to meet your needs\. If you chose to use an existing VPC, select one or more subnets in that VPC to use for your cluster\.

   1. For **Security group**, select the security group to attach to the container instances in your cluster\. If you choose to create a new security group, you can specify a CIDR block to allow inbound traffic from\. The default port 0\.0\.0\.0/0 is open to the internet\. You can also select a single port or a range of contiguous ports to open on the container instance\. For more complicated security group rules, you can choose an existing security group that you have already created\.
**Note**  
You can also choose to create a new security group and then modify the rules after the cluster is created\. For more information, see [Amazon EC2 Security Groups for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

   1. In the **Container instance IAM role** section, select the IAM role to use with your container instances\. If your account has the **ecsInstanceRole** that is created for you in the console first\-run wizard, it is selected by default\. If you do not have this role in your account, you can choose to create the role, or you can choose another IAM role to use with your container instances\.
**Important**  
The IAM role you use must have the `AmazonEC2ContainerServiceforEC2Role` managed policy attached to it, otherwise you will receive an error during cluster creation\. If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent does not connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. If you chose the Spot Instance type earlier, the **Spot Fleet Role IAM role** section indicates that an IAM role `ecsSpotFleetRole` is created\.

   1. In the **Tags** section, specify the key and value for each tag to associate with the cluster\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

   1. In the **CloudWatch Container Insights** section, choose whether to enable Container Insights for the cluster\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.

   1. Choose **Create**\.