# Creating a Network Load Balancer<a name="create-network-load-balancer"></a>

This section walks you through the process of creating a Network Load Balancer in the AWS Management Console\.

## Define your load balancer<a name="nlb-define-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and a listener\.

A *listener* is a process that checks for connection requests\. It is configured with a protocol and port for the frontend \(client to load balancer\) connections, and a protocol and port for the backend \(load balancer to backend instance\) connections\. In this example, you configure an Internet\-facing load balancer in the selected network with a listener that receives TCP traffic on port 80\.

**To define your load balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a region for your load balancer\. Be sure to select the same region that you selected for your Amazon ECS container instances\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. On the **Select load balancer type** page, choose **Create** under **Network Load Balancer**\.

1. Complete the **Configure Load Balancer** page as follows:

   1. For **Name**, type a name for your load balancer\.

   1. For **Scheme**, choose either **internet\-facing** or **internal**\. An internet\-facing load balancer routes requests from clients over the internet to targets\. An internal load balancer routes requests to targets using private IP addresses\.

   1. For **Listeners**, the default is a listener that accepts TCP traffic on port 80\. You can keep the default listener settings, modify the protocol or port of the listener, or choose **Add listener** to add another listener\.

   1. For **Availability Zones**, select the VPC that you used for your Amazon EC2 instances\. For each Availability Zone that you used to launch your Amazon EC2 instances, select an Availability Zone and then select the public subnet for that Availability Zone\. To associate an Elastic IP address with the subnet, select it from **Elastic IP**\.

   1. Choose **Next: Configure Routing**\.

## Configure routing<a name="nlb-configure-routing"></a>

You register targets, such as Amazon EC2 instances, with a target group\. The target group that you configure in this step is used as the target group in the listener rule, which forwards requests to the target group\. For more information, see [Target Groups for Your Network Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-target-groups.html) in the *User Guide for Network Load Balancers*\.

**To configure your target group**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the target group\.

1. Set **Protocol** and **Port** as needed\.

1. For **Target type**, choose whether to register your targets with an instance ID or an IP address\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.   
You cannot register instances by instance ID if they have the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, and T1\. You can register instances of these types by IP address\. 

1. For **Health checks**, keep the default health check settings\.

1. Choose **Next: Register Targets**\.

## Register targets with the target group<a name="nlb-register-targets"></a>

Your load balancer distributes traffic between the targets that are registered to its target groups\. When you associate a target group to an Amazon ECS service, Amazon ECS automatically registers and deregisters containers with your target group\. Because Amazon ECS handles target registration, you do not add targets to your target group at this time\.

**To skip target registration**

1. In the **Registered instances** section, ensure that no instances are selected for registration\.

1. Choose **Next: Review** to go to the next page in the wizard\.

## Review and create<a name="nlb-review"></a>

Review your load balancer and target group configuration and choose **Create** to create your load balancer\.

## Create an Amazon ECS service<a name="nlb-create-service"></a>

After your load balancer and target group are created, you can specify the target group in a service definition when you create a service\. When each task for your service is started, the container and port combination specified in the service definition is registered with your target group and traffic is routed from the load balancer to that container\. For more information, see [Creating a service](create-service.md)\.