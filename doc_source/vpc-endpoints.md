# Amazon ECS interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon ECS to use an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon ECS APIs by using private IP addresses\. PrivateLink restricts all network traffic between your VPC and Amazon ECS to the Amazon network\. You don't need an internet gateway, a NAT device, or a virtual private gateway\.

For more information about AWS PrivateLink and VPC endpoints, see [VPC Endpoints ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.

## Considerations for Amazon ECS VPC endpoints<a name="ecs-vpc-endpoint-considerations"></a>

Before you set up interface VPC endpoints for Amazon ECS, be aware of the following considerations:
+ Tasks using the Fargate launch type don't require the interface VPC endpoints for Amazon ECS, but you might need interface VPC endpoints for Amazon ECR, Secrets Manager, or Amazon CloudWatch Logs described in the following points\.
  + To allow your tasks to pull private images from Amazon ECR, you must create the interface VPC endpoints for Amazon ECR\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html) in the *Amazon Elastic Container Registry User Guide*\.
**Important**  
If you configure Amazon ECR to use an interface VPC endpoint, you can create a task execution role that includes condition keys to restrict access to a specific VPC or VPC endpoint\. For more information, see [Optional IAM permissions for Fargate tasks pulling Amazon ECR images over interface endpoints](task_execution_IAM_role.md#task-execution-ecr-conditionkeys)\.
  + To allow your tasks to pull sensitive data from Secrets Manager, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
  + If your VPC doesn't have an internet gateway and your tasks use the `awslogs` log driver to send log information to CloudWatch Logs, you must create an interface VPC endpoint for CloudWatch Logs\. For more information, see [Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html) in the *Amazon CloudWatch Logs User Guide*\.
+ Tasks using the EC2 launch type require that the container instances that they're launched on to run version `1.25.1` or later of the Amazon ECS container agent\. For more information, see [Amazon ECS Container Agent Versions](ecs-agent-versions.md)\.
+ VPC endpoints currently don't support cross\-Region requests\. Ensure that you create your endpoint in the same Region where you plan to issue your API calls to Amazon ECS\.
+ VPC endpoints only support Amazon\-provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the VPC\.
+ Controlling access to Amazon ECS by attaching an endpoint policy to the VPC endpoint isn't currently supported\. By default, full access to the service will be allowed through the endpoint\. For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

## Creating the VPC Endpoints for Amazon ECS<a name="ecs-setting-up-vpc-create"></a>

To create the VPC endpoint for the Amazon ECS service, use the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure in the *Amazon VPC User Guide* to create the following endpoints\. If you have existing container instances within your VPC, you should create the endpoints in the order that they're listed\. If you plan on creating your container instances after your VPC endpoint is created, the order doesn't matter\.
+ `com.amazonaws.region.ecs-agent`
+ `com.amazonaws.region.ecs-telemetry`
+ `com.amazonaws.region.ecs`

**Note**  
*region* represents the Region identifier for an AWS Region supported by Amazon ECS, such as `us-east-2` for the US East \(Ohio\) Region\.

If you have existing tasks that are using the EC2 launch type, after you have created the VPC endpoints, each container instance needs to pick up the new configuration\. For this to happen, you must either reboot each container instance or restart the Amazon ECS container agent on each container instance\. To restart the container agent, do the following\.<a name="procedure_restart_ecs_agent"></a>

**To restart the Amazon ECS container agent**

1. Log in to your container instance via SSH\. For more information, see [Connect to your container instance](instance-connect.md)\.

1. Stop the container agent\.

   ```
   sudo docker stop ecs-agent
   ```

1. Start the container agent\.

   ```
   sudo docker start ecs-agent
   ```

After you have created the VPC endpoints and restarted the Amazon ECS container agent on each container instance, all newly launched tasks pick up the new configuration\.

## Create the Secrets Manager and Systems Manager endpoints<a name="ecs-setting-up-secrets"></a>

If you are referencing either Secrets Manager secrets or Systems Manager Parameter Store parameters in your task definitions to inject sensitive data into your containers, you need to create the interface VPC endpoints for Secrets Manager or Systems Manager so those tasks can reach those services\. You only need to create the endpoints from the specific service your sensitive data is hosted in\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.

For more information about Secrets Manager VPC endpoints, see [Using Secrets Manager with VPC endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.

For more information about Systems Manager VPC endpoints, see [Using Systems Manager with VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) in the *AWS Systems Manager User Guide*\.