# Step 2: Configure a Network<a name="service-configure-network"></a>

If your service's task definition uses the `awsvpc` network mode, you must configure a VPC, subnet, and security group settings for your service\.

If your service's task definition does not use the `awsvpc` network mode, you can move on to the next step, [Step 3: Configuring Your Service to Use a Load Balancer](service-create-loadbalancer.md)\.

**To configure VPC and security group settings for your service**

1. If you have not done so already, follow the basic service configuration procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. For **Cluster VPC**, if you selected the EC2 launch type, choose the VPC in which your container instances reside\. If you selected the Fargate launch type, select the VPC that the Fargate tasks should use\. Ensure that the VPC you choose is not configured to require dedicated hardware tenancy, as that is not supported by Fargate tasks\.

1. For **Subnets**, choose the available subnets for your service task placement\.
**Important**  
Only private subnets are supported for the `awsvpc` network mode\. Because tasks do not receive public IP addresses, a NAT gateway is required for outbound internet access\. Inbound internet traffic should be routed through a load balancer\.

1. For **Security groups**, a security group has been created for your service's tasks, which allows HTTP traffic from the internet \(0\.0\.0\.0/0\)\. To edit the name or the rules of this security group, or to choose an existing security group, choose **Edit** and then modify your security group settings\.

1. For **Auto\-assign Public IP**, choose whether to have your tasks receive a public IP address\. If you are using Fargate tasks, a public IP address needs to be assigned to the task's elastic network interface,\. The network interface must have a route to the internet or a NAT gateway that can route requests to the internet, for the task to pull container images\.

1. If you are configuring your service to use a load balancer or if you are using the green/blue deployment type, continue to [Step 3: Configuring Your Service to Use a Load Balancer](service-create-loadbalancer.md)\. If you are not configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [Step 5: \(Optional\) Configuring Your Service to Use Service Auto Scaling](service-configure-auto-scaling.md)\.