# Creating a Classic Load Balancer<a name="create-standard-load-balancer"></a>

This section walks you through the process of creating a Classic Load Balancer in the AWS Management Console\.

You can create your Classic Load Balancer for use with EC2\-Classic or a VPC\. Some of the tasks described in these procedures apply only to load balancers in a VPC\.

## Define your load balancer<a name="define-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and a listener\.

A *listener* is a process that checks for connection requests\. It is configured with a protocol and port for the frontend \(client to load balancer\) connections and a protocol, and a protocol and port for the backend \(load balancer to backend instance\) connections\. In this example, you configure a listener that accepts HTTP requests on port 80 and sends them to the backend instances on port 80 using HTTP\.

**To define your load balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a region for your load balancer\. Be sure to select the same region that you selected for your Amazon ECS container instances\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. On the **Select load balancer type** page, choose **Classic Load Balancer**\.

1. For **Load Balancer name**, enter a unique name for your load balancer\.

   The load balancer name you choose must be unique within your set of load balancers, must have a maximum of 32 characters, and must only contain alphanumeric characters or hyphens\.

1. For **Create LB inside**, select the same network that your container instances are located in: EC2\-Classic or a specific VPC\.

1. The default values configure an HTTP load balancer that forwards traffic from port 80 at the load balancer to port 80 of your container instances, but you can modify these values for your application\. For more information, see [Listeners for Your Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html) in the *User Guide for Classic Load Balancers*\.

1. \[EC2\-VPC\] To improve the availability of your load balancer, select at least two subnets in different Availability Zones\. Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\. In the **Select Subnets** section, under **Available Subnets**, select the subnets\. The subnets that you select are moved under **Selected Subnets**\.
**Note**  
If you selected EC2\-Classic as your network, or you have a default VPC but did not choose **Enable advanced VPC configuration**, you do not see **Select Subnets**\.  
![\[Selected subnets\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/AddInstanceVPC_SelectedSubnet.png)

1. Choose **Next: Assign Security Groups** to go to the next page in the wizard\.

## Assign a security group to your load balancer in a VPC<a name="select-vpc-security-group"></a>

If you created your load balancer in a VPC, you must assign it a security group that allows inbound traffic to the ports that you specified for your load balancer and the health checks for your load balancer\. Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**Note**  
If you selected EC2\-Classic as your network, you do not see this page in the wizard and you can go to the next step\. Elastic Load Balancing provides a security group that is assigned to your load balancer for EC2\-Classic automatically\.

**To assign a security group to your load balancer**

1. On the **Assign Security Groups** page, choose **Create a new security group**\.

1. Enter a name and description for your security group, or leave the default name and description\. This new security group contains a rule that allows traffic to the port that you configured your load balancer to use\. If you specified a different port for the health checks, you must choose **Add Rule** to add a rule that allows inbound traffic to that port as well\.
**Note**  
Also assign this security group to container instances in your service, or another security group with the same rules\.  
![\[Select security groups\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/AddInstanceVPC_SGroups.png)

1. Choose **Next: Configure Security Settings** to go to the next page in the wizard\.

## Configure security settings<a name="configure-security-settings"></a>

For this tutorial, you can choose **Next: Configure Health Check** to continue to the next step\. For more information about creating an HTTPS load balancer and using additional security features, see [HTTPS Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-https-load-balancers.html) in the *User Guide for Classic Load Balancers*\.

## Configure health checks for your Amazon EC2 instances<a name="configure-health-check"></a>

Elastic Load Balancing automatically checks the health of the tasks in your service\. If Elastic Load Balancing finds an unhealthy task, it stops sending traffic to the Amazon EC2 instance hosting that task and reroutes the traffic to a healthy instance\.

**Note**  
The following procedure configures an HTTP \(port 80\) load balancer, but you can modify these values for your application\.

**To configure a health check for your instances**

1. On the **Configure Health Check** page, do the following:

   1. Leave **Ping Protocol** set to its default value of `HTTP`\.

   1. Leave **Ping Port** set to its default value of `80`\.

   1. For **Ping Path**, replace the default value with a single forward slash \("/"\)\. This tells Elastic Load Balancing to send health check queries to the default home page for your web server, such as `index.html` or `default.html`\.

   1. Leave the other fields at their default values\.  
![\[Configure health check\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/DefineLB_HealthCheck.png)

1. Choose **Next: Add EC2 Instances** to go to the next page in the wizard\.

## Load balancer instance registration<a name="register-instances"></a>

Your load balancer distributes traffic between the instances that are registered to it\. When you assign your load balancer to an Amazon ECS service, Amazon ECS automatically registers and deregisters container instances when tasks from your service are running on them\. Because Amazon ECS handles container instance registration, you do not add container instances to your load balancer at this time\.

**To skip instance registration and tag the load balancer**

1. On the **Add EC2 Instances** page, for **Add Instances to Load Balancer**, ensure that no instances are selected for registration\.

1. Leave the other fields at their default values\.

1. Choose **Next: Add Tags** to go to the next page in the wizard\.

## Tag your load balancer<a name="elb-add-tags"></a>

You can tag your load balancer, or continue to the next step\. You can tag your load balancer later on\. For more information, see [Tag Your Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/add-remove-tags.html) in the *User Guide for Classic Load Balancers*\.

**To add tags to your load balancer**

1. On the **Add Tags** page, specify a key and a value for the tag\.

1. To add another tag, choose **Create Tag** and specify a key and a value for the tag\.

1. After you are finished adding tags, choose **Review and Create**\.

## Create and verify your load balancer<a name="verify-load-balancer"></a>

Before you create the load balancer, review the settings that you selected\. After creating the load balancer, you can create a service that uses it to verify that it's sending traffic to your container instances\.

**To finish creating your load balancer**

1. On the **Review** page, check your settings\. To change the initial settings, choose the corresponding edit link\.

1. Choose **Create** to create your load balancer\.

1. After you are notified that your load balancer was created, choose **Close**\.

## Create an Amazon ECS service<a name="load-balancer-create-service"></a>

After your load balancer is created, you can specify it in a service definition when you create a service\. For more information, see [Creating a service](create-service.md)\.