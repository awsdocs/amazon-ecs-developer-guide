# Creating an Application Load Balancer<a name="create-application-load-balancer"></a>

This section walks you through the process of creating an Application Load Balancer in the AWS Management Console\. For information about how to create an Application Load Balancer using the AWS CLI, see [Tutorial: Create an Application Load Balancer using the AWS CLI](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-application-load-balancer-cli.html) in the *User Guide for Application Load Balancers*\.

This section walks you through the process of creating an Application Load Balancer in the AWS Management Console\. For information about how to create an Application Load Balancer using the AWS CLI, see [Tutorial: Create an Application Load Balancer using the AWS CLI](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-application-load-balancer-cli.html) in the *User Guide for Application Load Balancers*\.

## Configure a target group for routing<a name="alb-configure-routing"></a>

In this section, you create a target group for your load balancer and the health check criteria for targets that are registered within that group\.

Each target group is used to route requests to one or more registered targets\. When a rule condition is met, traffic is forwarded to the corresponding target group\. 

Your load balancer distributes traffic between the targets that are registered to its target groups\. When you associate a target group to an Amazon ECS service, Amazon ECS automatically registers and deregisters containers with your target group\. Because Amazon ECS handles target registration, you do not add targets to your target group at this time\.

**To create a target group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. For **Choose a target type**, select the target type\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), you must choose **IP addresses** as the target type This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. For **Target group name**, type a name for the target group\. This name must be unique per region per account, can have a maximum of 32 characters, must contain only alphanumeric characters or hyphens, and must not begin or end with a hyphen\.

1. \(Optional\) For **Protocol** and **Port**, modify the default values as needed\.

1. If the target type is **IP addresses**, choose **IPv4**\.

1. For **VPC**, select a virtual private cloud \(VPC\)\. Note that for **IP addresses** target types, the VPCs available for selection are those that support the **IP address type** that you chose in the previous step\. 

1. \(Optional\) For **Protocol version**, leave the default\.

1. \(Optional\) In the **Health checks** section, keep the default settings\.

1. \(Optional\) Add one or more tags as follows:

   1. Expand the **Tags** section\.

   1. Choose **Add tag**\.

   1. Enter the tag key and the tag value\.

1. Choose **Next**\.

1. Choose **Create target group**\.

## Define your load balancer<a name="alb-define-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and a listener\.

A *listener* is a process that checks for connection requests\. It is configured with a protocol and a port for the frontend \(client to load balancer\) connections, and protocol and a port for the backend \(load balancer to backend instance\) connections\. In this example, you configure a listener that accepts HTTP requests on port 80 and sends them to the containers in your tasks on port 80 using HTTP\.

**To configure your load balancer and listener**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. Under **Application Load Balancer**, choose **Create**\.

1. Under **Basic configuration**, do the following:

   1. For **Load balancer name**, enter a name for your load balancer\. For example, **my\-alb**\. The name of your Application Load Balancer must be unique within your set of Application Load Balancers and Network Load Balancers for the Region\. Names can have a maximum of 32 characters, and can contain only alphanumeric characters and hyphens\. They cannot begin or end with a hyphen, or with `internal-`\.

   1. For **Scheme**, choose **Internet\-facing** or **Internal**\. An internet\-facing load balancer routes requests from clients to targets over the internet\. An internal load balancer routes requests to targets using private IP addresses\.

   1. For **IP address type**, choose **IPv4** or **Dualstack**\. Use **IPv4** if your clients use IPv4 addresses to communicate with the load balancer\. Choose **Dualstack** if your clients use both IPv4 and IPv6 addresses to communicate with the load balancer\. 

1. Under **Network mapping**, do the following:

   1. For **VPC**, select the same VPC that you used for the container instances on which you intend to run your service\.

   1. For **Mappings**, select the check box for the Availability Zones to use for your load balancer\. If there is one subnet for that Availability Zone, it is selected\. If there is more than one subnet for that Availability Zone, select one of the subnets\. You can select only one subnet per Availability Zone\. Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.

1. Under **Security groups**, do the following:

   For **Security groups**, select an existing security group, or create a new one\. 

   The security group for your load balancer must allow it to communicate with registered targets on both the listener port and the health check port\. The console can create a security group for your load balancer on your behalf with rules that allow this communication\. You can also create a security group and select it instead\. For information about how to create a security group, see [Security groups for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-update-security-groups.html) in *Elastic Load Balancing Application Load Balancers*\.

   \(Optional\) To create a new security group for your load balancer, choose **Create a new security group**\.

1. Under **Listeners and routing**, do the following:

   The default listener accepts HTTP traffic on port 80\. You can keep the default protocol and port\. For **Default action**, choose the target group that you created\. You can optionally choose **Add listener** to add another listener \(for example, an HTTPS listener\)\.

   If you create an HTTPS listener, configure the required 

   When you use HTTPS for your load balancer listener, you must deploy an SSL certificate on your load balancer\. The load balancer uses this certificate to terminate the connection and decrypt requests from clients before sending them to the targets\. Under **Secure listener settings**, do the following: 

   1. For **Select policy**, choose a predefined security policy\. For details on the security policies, see [Security Policies](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html#describe-ssl-policies) in *Elastic Load Balancing Application Load Balancers*\.

   1. For **Default SSL certificate**, do one of the following:
      + If you created or imported a certificate using AWS Certificate Manager, select **From ACM**, and then select the certificate\.
      + If you uploaded a certificate using IAM, select **From IAM**, and then select the certificate\.
      + If you want to import a certificate to ACM or IAM , enter a certificate name\. Then, paste the PEM\-encoded private key and body\.

1. \(Optional\) You can use **Add\-on services**, such as the **AWS Global Accelerator** to create an accelerator and associate the load balancer with the accelerator\. The accelerator name can have up to 64 characters\. Allowed characters are a\-z, A\-Z, 0\-9, \. and \- \(hyphen\)\. After the accelerator is created, you can use the **AWS Global Accelerator** console to manage it\. 

1. \(Optional\) Tag your Application Load Balancer\. Under **Tag and create**, do the following

   1. Expand the **Tags** section\.

   1. Choose **Add tag**\.

   1. Enter the tag key and the tag value\.

1. Review your configuration, and choose **Create load balancer**\. 

## Create a security group rule for your container instances<a name="alb-sec-group"></a>

After your Application Load Balancer has been created, you must add an inbound rule to your container instance security group that allows traffic from your load balancer to reach the containers\.

**To allow inbound traffic from your load balancer to your container instances**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation, choose **Security Groups**\.

1. Choose the security group that your container instances use\. If you created your container instances by using the Amazon ECS first run wizard, this security group may have the description, **ECS Allowed Ports**\.

1. Choose the **Inbound** tab, and then choose **Edit inbound rules**\.

1. For **Type**, choose **All traffic**\.

1. For **Source**, choose **Custom**, and then select the Application Load Balancer security group\. This rule allows all traffic from your Application Load Balancer to reach the containers in your tasks that are registered with your load balancer\. 

1. Choose **Save** to finish\.

## Create an Amazon ECS service<a name="alb-create-service"></a>

After your load balancer and target group are created, you can specify the target group in a service definition when you create a service\. When each task for your service is started, the container and port combination specified in the service definition is registered with your target group and traffic is routed from the load balancer to that container\. For more information, see [Creating an Amazon ECS service](create-service.md)\.