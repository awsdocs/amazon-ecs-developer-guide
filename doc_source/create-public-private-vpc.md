# Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters<a name="create-public-private-vpc"></a>

Container instances in your clusters need external network access to communicate with the Amazon ECS service endpoint\. However, you might have tasks and services that you would like to run in private subnets\. Creating a VPC with both public and private subnets provides you the flexibility to launch tasks and services in either a public or private subnet\. Tasks and services in the private subnets can access the internet through a NAT gateway\. Services in both the public and private subnets can be configured to use a load balancer so that they can still be reached from the public internet\.

This tutorial guides you through creating a VPC with two public subnets and two private subnets, which are provided with internet access through a NAT gateway\.

## Step 1: Create an Elastic IP Address for Your NAT Gateway<a name="create-EIP"></a>

A NAT gateway requires an Elastic IP address in your public subnet, but the VPC wizard does not create one for you\. Create the Elastic IP address before running the VPC wizard\.

**To create an Elastic IP address**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left navigation pane, choose **Elastic IPs**\.

1. Choose **Allocate new address**, **Allocate**, **Close**\.

1. Note the **Allocation ID** for your newly created Elastic IP address; you enter this later in the VPC wizard\.

## Step 2: Run the VPC Wizard<a name="run-VPC-wizard"></a>

The VPC wizard automatically creates and configures most of your VPC resources for you\.

**To run the VPC wizard**

1. In the left navigation pane, choose **VPC Dashboard**\.

1. Choose **Launch VPC Wizard**, **VPC with Public and Private Subnets**, **Select**\.

1. For **VPC name**, give your VPC a unique name\.

1. For **Elastic IP Allocation ID**, choose the ID of the Elastic IP address that you created earlier\.

1. Choose **Create VPC**\.

1. When the wizard is finished, choose **OK**\. Note the Availability Zone in which your VPC subnets were created\. Your additional subnets should be created in a different Availability Zone\.

   Non\-default subnets, such as those created by the VPC wizard, are not auto\-assigned public IPv4 addresses\. Instances launched in the public subnet must be assigned a public IPv4 address to communicate with the Amazon ECS service endpoint\.

**To modify your public subnet's IPv4 addressing behavior**

1. In the left navigation pane, choose **Subnets**\.

1. Select the public subnet for your VPC\. By default, the name created by the VPC wizard is **Public subnet**\.

1. Choose **Actions**, **Modify auto\-assign IP settings**\.

1. Select the **Enable auto\-assign public IPv4 address** check box, and then choose **Save**\.

## Step 3: Create Additional Subnets<a name="create-add-subnets"></a>

The wizard creates a VPC with a single public and a single private subnet in a single Availability Zone\. For greater availability, you should create at least one more of each subnet type in a different Availability Zone so that your VPC has both public and private subnets across two Availability Zones\.

**To create an additional private subnet**

1. In the left navigation pane, choose **Subnets**\.

1. Choose **Create Subnet**\.

1. For **Name tag**, enter a name for your subnet, such as **Private subnet**\.

1. For **VPC**, choose the VPC that you created earlier\.

1. For **Availability Zone**, choose a different Availability Zone than your original subnets in the VPC\.

1. For **IPv4 CIDR block**, enter a valid CIDR block\. For example, the wizard creates CIDR blocks in 10\.0\.0\.0/24 and 10\.0\.1\.0/24 by default\. You could use **10\.0\.3\.0/24** for your second private subnet\.

1. Choose **Yes, Create**\.

**To create an additional public subnet**

1. In the left navigation pane, choose **Subnets** and then **Create Subnet**\.

1. For **Name tag**, enter a name for your subnet, such as **Public subnet**\.

1. For **VPC**, choose the VPC that you created earlier\.

1. For **Availability Zone**, choose the same Availability Zone as the additional private subnet that you created in the previous procedure\.

1. For **IPv4 CIDR block**, enter a valid CIDR block\. For example, the wizard creates CIDR blocks in 10\.0\.0\.0/24 and 10\.0\.1\.0/24 by default\. You could use **10\.0\.2\.0/24** for your second public subnet\.

1. Choose **Yes, Create**\.

1. Select the public subnet that you just created and choose **Route Table**, **Edit**\.

1. By default, the main route table is selected\. Choose the other available route table so that the **0\.0\.0\.0/0** destination is routed to the internet gateway \(**igw\-*xxxxxxxx***\) and choose **Save**\.

1. With your second public subnet still selected, choose **Subnet Actions**, **Modify auto\-assign IP settings**\.

1. Select **Enable auto\-assign public IPv4 address** and choose **Save**, **Close**\.

## Next Steps<a name="vpc-next-steps"></a>

After you have created your VPC, you should consider the following next steps:
+ Create security groups for your public and private resources if they require inbound network access\. For more information, see [Working with Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#WorkingWithSecurityGroups) in the *Amazon VPC User Guide*\.
+ Create Amazon ECS clusters in your private or public subnets\. For more information, see [Creating a cluster](create_cluster.md)\. If you use the cluster creation wizard in the Amazon ECS console, you can specify the VPC that you just created and the public or private subnets in which to launch your instances, depending on your use case\.
  + To make your containers directly accessible from the internet, launch instances into your *public* subnets\. Be sure to configure your container instance security groups appropriately\.
  + To avoid making containers directly accessible from the internet, launch instances into your *private* subnets\.
+ Create a load balancer in your public subnets that can route traffic to services in your public or private subnets\. For more information, see [Service load balancing](service-load-balancing.md)\.