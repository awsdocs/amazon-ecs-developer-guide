# Amazon ECS interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon ECS to use an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that allows you to privately access Amazon ECS APIs by using private IP addresses\. AWS PrivateLink restricts all network traffic between your VPC and Amazon ECS to the Amazon network\. You don't need an internet gateway, a NAT device, or a virtual private gateway\.

For more information about AWS PrivateLink and VPC endpoints, see [VPC Endpoints ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.

## Considerations for Amazon ECS VPC endpoints<a name="ecs-vpc-endpoint-considerations"></a>

### Considerations for Amazon ECS VPC endpoints for the Fargate launch type<a name="fargate-ecs-vpc-endpoint-considerations"></a>

Before you set up interface VPC endpoints for Amazon ECS, be aware of the following considerations:
+ Tasks using the Fargate launch type don't require the interface VPC endpoints for Amazon ECS, but you might need interface VPC endpoints for Amazon ECR, Secrets Manager, or Amazon CloudWatch Logs described in the following points\.
  + To allow your tasks to pull private images from Amazon ECR, you must create the interface VPC endpoints for Amazon ECR\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html) in the *Amazon Elastic Container Registry User Guide*\.

    If your VPC doesn't have an internet gateway, you must create the gateway endpoint for Amazon S3\. For more information, see [Create the Amazon S3 gateway endpoint](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html#ecr-setting-up-s3-gateway) in the *Amazon Elastic Container Registry User Guide*\. The interface endpoints for Amazon S3 can't be used with Amazon ECR\.
**Important**  
If you configure Amazon ECR to use an interface VPC endpoint, you can create a task execution role that includes condition keys to restrict access to a specific VPC or VPC endpoint\. For more information, see [Optional IAM permissions for Fargate tasks pulling Amazon ECR images over interface endpoints](task_execution_IAM_role.md#task-execution-ecr-conditionkeys)\.
  + To allow your tasks to pull sensitive data from Secrets Manager, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
  + If your VPC doesn't have an internet gateway and your tasks use the `awslogs` log driver to send log information to CloudWatch Logs, you must create an interface VPC endpoint for CloudWatch Logs\. For more information, see [Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html) in the *Amazon CloudWatch Logs User Guide*\.
+ VPC endpoints currently don't support cross\-Region requests\. Ensure that you create your endpoint in the same Region where you plan to issue your API calls to Amazon ECS\. For example, assume that you want to run tasks in US East \(N\. Virginia\)\. Then, you must create the Amazon ECS VPC endpoint in US East \(N\. Virginia\)\. An Amazon ECS VPC endpoint created in any other region can't run tasks in US East \(N\. Virginia\)\.
+ VPC endpoints only support Amazon\-provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ The security group attached to the VPC endpoint must allow incoming connections on TCP port 443 from the private subnet of the VPC\.
+ Service Connect management of the Envoy proxy uses the `com.amazonaws.region.ecs-agent` VPC endpoint\. When you don't use the VPC endpoints, Service Connect management of the Envoy proxy uses the `ecs-sc` endpoint in that Region\. For a list of the Amazon ECS endpoints in each Region, see   [Amazon ECS endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ecs-service.html)\.  

### Considerations for Amazon ECS VPC endpoints for the EC2 launch type<a name="ec2-ecs-vpc-endpoint-considerations"></a>

Before you set up interface VPC endpoints for Amazon ECS, be aware of the following considerations:
+ Tasks using the EC2 launch type require that the container instances that they're launched on to run version `1.25.1` or later of the Amazon ECS container agent\. For more information, see [Amazon ECS Linux container agent versions](ecs-agent-versions.md)\.
+ To allow your tasks to pull sensitive data from Secrets Manager, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
+ If your VPC doesn't have an internet gateway and your tasks use the `awslogs` log driver to send log information to CloudWatch Logs, you must create an interface VPC endpoint for CloudWatch Logs\. For more information, see [Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html) in the *Amazon CloudWatch Logs User Guide*\.
+ VPC endpoints currently don't support cross\-Region requests\. Ensure that you create your endpoint in the same Region where you plan to issue your API calls to Amazon ECS\. For example, assume that you want to run tasks in US East \(N\. Virginia\)\. Then, you must create the Amazon ECS VPC endpoint in US East \(N\. Virginia\)\. An Amazon ECS VPC endpoint created in any other region can't run tasks in US East \(N\. Virginia\)\.
+ VPC endpoints only support Amazon\-provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ The security group attached to the VPC endpoint must allow incoming connections on TCP port 443 from the private subnet of the VPC\.

## Creating the VPC Endpoints for Amazon ECS<a name="ecs-setting-up-vpc-create"></a>

To create the VPC endpoint for the Amazon ECS service, use the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure in the *Amazon VPC User Guide* to create the following endpoints\. If you have existing container instances within your VPC, you should create the endpoints in the order that they're listed\. If you plan on creating your container instances after your VPC endpoint is created, the order doesn't matter\.
+ `com.amazonaws.region.ecs-agent`
+ `com.amazonaws.region.ecs-telemetry`
+ `com.amazonaws.region.ecs`

**Note**  
*region* represents the Region identifier for an AWS Region supported by Amazon ECS, such as `us-east-2` for the US East \(Ohio\) Region\.

If you have existing tasks that are using the EC2 launch type, after you have created the VPC endpoints, each container instance needs to pick up the new configuration\. For this to happen, you must either reboot each container instance or restart the Amazon ECS container agent on each container instance\. To restart the container agent, do the following\.<a name="procedure_restart_ecs_agent"></a>

**To restart the Amazon ECS container agent**

1. Log in to your container instance via SSH\.

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

If you are referencing either Secrets Manager secrets or Systems Manager Parameter Store parameters in your task definitions to inject sensitive data into your containers, you need to create the interface VPC endpoints for Secrets Manager or Systems Manager so those tasks can reach those services\. You only need to create the endpoints from the specific service your sensitive data is hosted in\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.

For more information about Secrets Manager VPC endpoints, see [Using Secrets Manager with VPC endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.

For more information about Systems Manager VPC endpoints, see [Using Systems Manager with VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) in the *AWS Systems Manager User Guide*\.

## Create the Systems Manager Session Manager VPC endpoints when using the ECS Exec feature<a name="ecs-vpc-endpoint-ecsexec"></a>

If you use the ECS Exec feature, you need to create the interface VPC endpoints for Systems Manager Session Manager\. For more information, see [Using Amazon ECS Exec for debugging](ecs-exec.md)\.

For more information about Systems Manager Session Manager VPC endpoints, see [Use AWS PrivateLink to set up a VPC endpoint for Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-privatelink.html) in the *AWS Systems Manager User Guide*\.

## Creating a VPC endpoint policy for Amazon ECS<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to Amazon ECS\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

**Example: VPC endpoint policy for Amazon ECS actions**  
The following is an example of an endpoint policy for Amazon ECS\. When attached to an endpoint, this policy grants access to permission to create and list clusters\. The `CreateCluster` and `ListClusters` actions do not accept any resources, so the resource definition is set to \* for all resources\. 

```
{
   "Statement":[
    {
      "Effect": "Allow",
      "Action": [
        "ecs:CreateCluster",
        "ecs:ListClusters"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```