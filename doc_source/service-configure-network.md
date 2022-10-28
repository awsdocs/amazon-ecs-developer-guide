# Step 2: Configure a network<a name="service-configure-network"></a>

If your service's task definition uses the `awsvpc` network mode, you must configure a VPC, subnet, and security group for your service\.

If your service's task definition uses the `bridge`, `host`, or `none` network modes, you can move on to the next step, [Step 3: Configuring your service to use a load balancer](service-create-loadbalancer.md)\.

For tasks that are hosted on Amazon EC2 instances, the `awsvpc` network mode doesn't provide task ENIs with public IP addresses\. To access the internet, tasks that are hosted on Amazon EC2 instances can be launched in a private subnet that's configured to use a NAT gateway\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. Inbound network access must be from within the VPC using the private IP address or DNS hostname, or routed through a load balancer from within the VPC\. Tasks that are launched within public subnets don't have internet access\.

**To configure VPC and security group settings for your service**

1. If you host tasks on EC2 instances, for **Cluster VPC**, choose the VPC that your instances are in\. 

   If you host tasks on Fargate, or **Cluster VPC**, choose the VPC that the Amazon ECS on Fargate tasks use\. The VPC cannot be configured to require dedicated hardware tenancy, because Fargate does not support the feature\.

1. For **Subnets**, choose the available subnets for your service task placement\.

1. For **Security groups**, choose a security group was created for your service's tasks\. This security group allows HTTP traffic access from the internet \(`0.0.0.0/0`\)\. To edit the name or the rules of this security group or to choose an existing security group, choose **Edit** and then modify your security group settings\.

1. For **Auto\-assign Public IP**, choose whether to have your tasks receive a public IP address\. For tasks on Fargate, for the task to pull the container image, it must either use a public subnet and be assigned a public IP address or a private subnet that has a route to the internet or a NAT gateway that can route requests to the internet\.

1. If you're configuring your service to use a load balancer or if you're using the blue/green deployment type, continue to [Step 3: Configuring your service to use a load balancer](service-create-loadbalancer.md)\. If you aren't configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)\.