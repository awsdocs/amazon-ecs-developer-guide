# Interface VPC Endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon ECS to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon ECS APIs by using private IP addresses\. PrivateLink restricts all network traffic between your VPC and Amazon ECS to the Amazon network\. Also, you don't need an Internet gateway, a NAT device, or a virtual private gateway\.

You are not required to configure PrivateLink, but it is recommended\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)\.

## Before You Begin<a name="ecs-setting-up-vpc-considerations"></a>

Before you configure VPC endpoints for Amazon ECS, be aware of the following considerations\.
+ VPC endpoints currently do not support cross\-region requests\. Ensure that you create your endpoint in the same region in which you plan to issue your API calls to Amazon ECS\.
+ VPC endpoints only support Amazon\-provided DNS through Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the Amazon VPC User Guide\.
+ The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the VPC\.
+ Controlling access to Amazon ECS by attaching an endpont policy to the VPC endpoint is not currently supported\. By default, full access to the service will be allowed through the endpoint\. For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the Amazon VPC User Guide\.

## Creating the VPC EndPoint for Amazon ECS<a name="ecs-setting-up-vpc-create"></a>

To create the VPC endpoint for the Amazon ECS service, use the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure in the Amazon VPC User Guide to create the following endpoints\. If you have existing container instances within your VPC, you should create the endpoints in the order they are listed\. If you plan on creating your container instances after your VPC endpoint is created, then the order does not matter\.
+ `com.amazonaws.region.ecs-agent`
+ `com.amazonaws.region.ecs-telemetry`
+ `com.amazonaws.region.ecs`

**Note**  
*region* represents the region identifier for an AWS region supported by Amazon ECS, such as `us-east-2` for the US East \(Ohio\) Region\.

If you have existing tasks that are using the Fargate launch type, no further action is needed in order to take advantage of PrivateLink\.

If you have existing tasks that are using the EC2 launch type, after the VPC endpoints are created you must either reboot each container instance or restart the Amazon ECS container agent on each container instance in order for them to pick up the new configuration\. To restart the container agent, do the following\.<a name="procedure_restart_ecs_agent"></a>

**To restart the Amazon ECS container agent**

1. Log in to your container instance via SSH\. For more information, see [Connect to Your Container Instance](instance-connect.md)\.

1. Stop the container agent\.

   ```
   sudo docker stop ecs-agent
   ```

1. Start the container agent\.

   ```
   sudo docker start ecs-agent
   ```

After the VPC endpoints are created and the Amazon ECS container agent has been restarted on each container instance, all newly launched tasks will pick up the new configuration\.