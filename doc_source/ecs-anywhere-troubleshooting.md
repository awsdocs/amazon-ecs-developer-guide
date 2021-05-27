# Troubleshooting ECS Anywhere issues<a name="ecs-anywhere-troubleshooting"></a>

ECS Anywhere provides support for registering an on\-premise server or virtual machine \(VM\), referred to as an *external instance*, to your Amazon ECS cluster\. The following are common issues you may have and general troubleshooting recommendations for them\.

**Topics**
+ [External instance registration issues](#ecs-anywhere-troubleshooting-registration)
+ [External instance network issues](#ecs-anywhere-troubleshooting-networking)
+ [Issues running tasks on your external instance](#ecs-anywhere-troubleshooting-runtask)

## External instance registration issues<a name="ecs-anywhere-troubleshooting-registration"></a>

When registering an external instance with your Amazon ECS cluster, the following requirements must be met\.
+ An Systems Manager activation, consisting of an *activation ID* and *activation code*, must be retrieved\. This is used to register the external instance as a Systems Manager managed instance\. When a Systems Manager activation is requested, you may optionally specify a registration limit and expiration date\. The registration limit specifies the maximum number of instances that can be registered using the activation and the default value is `1` instance\. The expiration date specifies when the activation expires and the default value is 24 hours\. If the Systems Manager activation you're using to register your external instance isn't valid, you should request a new one\. For more information, see [Registering an external instance to a cluster](ecs-anywhere-registration.md)\.
+ An IAM policy is used to provide your external instance the permissions needed to communicate with AWS APIs\. If this managed policy is not created properly and doesn't contain the required permissions, external instance registration will fail\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.
+ Amazon ECS provides an installation script which installs Docker, the Amazon ECS container agent, and the Systems Manager Agent on your external instance\. If the installation script fails, it is common that the script can't be run again on the same instance without an error\. If this happens, follow the cleanup process to clear AWS resources from the instance so you can run the installation script again\. For more information, see [Deregistering an external instance](ecs-anywhere-deregistration.md)\.
**Note**  
Be aware that if the installation script successfully requested and used the Systems Manager activation, that running the installation script a second time will use the Systems Manager activation again and you may run into the registration limit for that activation\.

## External instance network issues<a name="ecs-anywhere-troubleshooting-networking"></a>

Your external instance requires a network connection to AWS in order to communicate changes\. If your external instance loses its network connection to AWS, tasks running on your instances will continue to run unless stopped manually\. The AWS credentials used by the Amazon ECS container agent and Systems Manager Agent on the external instance will renew automatically once the connection to AWS is restored\. For more information about the AWS domains that are used for communication between your external instance and AWS, see [Networking with ECS Anywhere](ecs-anywhere.md#ecs-anywhere-networking)\.

## Issues running tasks on your external instance<a name="ecs-anywhere-troubleshooting-runtask"></a>

If your tasks or containers fail to run on your external instance, the most common causes are going to be either network or permission related\. If your containers are pulling their images from Amazon ECR or are configured to send container logs to CloudWatch Logs, your task definition must specify a valid task execution IAM role\. Without a valid task execution IAM role, your containers will fail to start\. For more information, see [Conditional IAM permissions](ecs-anywhere-iam.md#ecs-anywhere-iam-conditional)\. For more information about network related issues, see [External instance network issues](#ecs-anywhere-troubleshooting-networking)\.

**Important**  
Amazon ECS provides the Amazon ECS logs collection tool\. This tool makes it easy to collect logs from your external instances to assist in troubleshooting\. For more information, see [Amazon ECS logs collector](ecs-logs-collector.md)\.