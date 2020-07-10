# Creating an Application Load Balancer<a name="create-application-load-balancer"></a>

This section walks you through the process of creating an Application Load Balancer in the AWS Management Console\.

## Define your load balancer<a name="alb-define-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and a listener\.

A *listener* is a process that checks for connection requests\. It is configured with a protocol and a port for the frontend \(client to load balancer\) connections, and protocol and a port for the backend \(load balancer to backend instance\) connections\. In this example, you configure a listener that accepts HTTP requests on port 80 and sends them to the containers in your tasks on port 80 using HTTP\.

**To define your load balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a Region for your load balancer\. Be sure to select the same Region that you selected for your Amazon ECS container instances\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. On the **Select load balancer type** page, choose **Application Load Balancer** and then choose **Continue**\.

1. Complete the **Configure Load Balancer** page as follows:

   1. For **Name**, type a name for your load balancer\.

   1. For **Scheme**, an internet\-facing load balancer routes requests from clients over the internet to targets\. An internal load balancer routes requests to targets using private IP addresses\.

   1. For **IP address type**, choose **ipv4** to support IPv4 addresses only or **dualstack** to support both IPv4 and IPv6 addresses\.

   1. For **Listeners**, the default is a listener that accepts HTTP traffic on port 80\. You can keep the default listener settings, modify the protocol or port of the listener, or choose **Add** to add another listener\.
**Note**  
If you plan on routing traffic to more than one target group, see [ListenerRules](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html) for details on how to add host or path\-based rules\.

   1. For **VPC**, select the same VPC that you used for the container instances on which you intend to run your service\.

   1. For **Availability Zones**, select the check box for the Availability Zones to enable for your load balancer\. If there is one subnet for that Availability Zone, it is selected\. If there is more than one subnet for that Availability Zone, select one of the subnets\. You can select only one subnet per Availability Zone\. Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.

   1. Choose **Next: Configure Security Settings**\.

## Configure security settings<a name="alb-configure-security-settings"></a>

If you created a secure listener in the previous step, complete the **Configure Security Settings** page as follows; otherwise, choose **Next: Configure Security Groups**\.

**To configure security settings**

1. If you have a certificate from AWS Certificate Manager, choose **Choose an existing certificate from AWS Certificate Manager \(ACM\)**, and then choose the certificate from **Certificate name**\.

1. If you have already uploaded a certificate using IAM, choose **Choose an existing certificate from AWS Identity and Access Management \(IAM\)**, and then choose your certificate from **Certificate name**\.

1. If you have a certificate ready to upload, choose **Upload a new SSL Certificate to AWS Identity and Access Management \(IAM\)**\. For **Certificate name**, type a name for the certificate\. For **Private Key**, copy and paste the contents of the private key file \(PEM\-encoded\)\. In **Public Key Certificate**, copy and paste the contents of the public key certificate file \(PEM\-encoded\)\. In **Certificate Chain**, copy and paste the contents of the certificate chain file \(PEM\-encoded\), unless you are using a self\-signed certificate and it's not important that browsers implicitly accept the certificate\.

1. For **Select policy**, choose a predefined security policy\. For details on the security policies, see [Security Policies](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html#describe-ssl-policies) in the *User Guide for Application Load Balancers*\.

1. Choose **Next: Configure Security Groups**\.

## Configure security groups<a name="alb-configure-security-groups"></a>

You must assign a security group to your load balancer that allows inbound traffic to the ports that you specified for your listeners\. Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To assign a security group to your load balancer**

1. On the **Assign Security Groups** page, choose **Create a new security group**\.

1. Enter a name and description for your security group, or leave the default name and description\. This new security group contains a rule that allows traffic to the port that you configured your listener to use\.
**Note**  
Later in this topic, you create a security group rule for your container instances that allows traffic on all ports coming from the security group created here, so that the Application Load Balancer can route traffic to dynamically assigned host ports on your container instances\.  
![\[Select security groups\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alb-create-security-group.png)

1. Choose **Next: Configure Routing** to go to the next page in the wizard\.

## Configure routing<a name="alb-configure-routing"></a>

In this section, you create a target group for your load balancer and the health check criteria for targets that are registered within that group\.

**To create a target group and configure health checks**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the new target group\.

1. Set **Protocol** and **Port** as needed\.

1. For **Target type**, choose whether to register your targets with an instance ID or an IP address\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. For **Health checks**, keep the default health check settings\.

1. Choose **Next: Register Targets**\.

## Register targets<a name="alb-register-targets"></a>

Your load balancer distributes traffic between the targets that are registered to its target groups\. When you associate a target group to an Amazon ECS service, Amazon ECS automatically registers and deregisters containers with your target group\. Because Amazon ECS handles target registration, you do not add targets to your target group at this time\.

**To skip target registration**

1. In the **Registered instances** section, ensure that no instances are selected for registration\.

1. Choose **Next: Review** to go to the next page in the wizard\.

## Review and create<a name="alb-review"></a>

Review your load balancer and target group configuration and choose **Create** to create your load balancer\.

## Create a security group rule for your container instances<a name="alb-sec-group"></a>

After your Application Load Balancer has been created, you must add an inbound rule to your container instance security group that allows traffic from your load balancer to reach the containers\.

**To allow inbound traffic from your load balancer to your container instances**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation, choose **Security Groups**\.

1. Choose the security group that your container instances use\. If you created your container instances by using the Amazon ECS first run wizard, this security group may have the description, **ECS Allowed Ports**\.

1. Choose the **Inbound** tab, and then choose **Edit**\.

1. For **Type**, choose **All traffic**\.

1. For **Source**, choose **Custom**, and then type the name of your Application Load Balancer security group that you created in [Configure security groups](#alb-configure-security-groups)\. This rule allows all traffic from your Application Load Balancer to reach the containers in your tasks that are registered with your load balancer\.   
![\[Edit inbound rules\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/edit_inbound_rules.png)

1. Choose **Save** to finish\.

## Create an Amazon ECS service<a name="alb-create-service"></a>

After your load balancer and target group are created, you can specify the target group in a service definition when you create a service\. When each task for your service is started, the container and port combination specified in the service definition is registered with your target group and traffic is routed from the load balancer to that container\. For more information, see [Creating a service](create-service.md)\.