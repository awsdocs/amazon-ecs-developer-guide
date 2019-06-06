# Elastic Network Interface Trunking<a name="container-instance-eni"></a>

Each Amazon ECS task that uses the `awsvpc` network mode receives its own elastic network interface \(ENI\), which is attached to the container instance that hosts it\. There is a default limit to the number of network interfaces that can be attached to an Amazon EC2 instance, and the primary network interface counts as one\. For example, by default a `c5.large` instance may have up to three ENIs attached to it\. The primary network interface for the instance counts as one, so you can attach an additional two ENIs to the instance\. Because each task using the `awsvpc` network mode requires an ENI, you can typically only run two such tasks on this instance type\.

Amazon ECS supports launching container instances with increased ENI density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. This configuration allows you to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI limit of ten\. The eontainer instance will have the primary network interface and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. Neither the primary network interface or the trunk network interface counts against the ENI limit, so this configuration allows you to launch ten tasks on the container instance instead of the current two tasks\.

The trunk ENI is fully managed by Amazon ECS and is deleted when you either terminate or deregister your container instance from the cluster\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

## ENI Trunking Considerations<a name="eni-trunking-considerations"></a>

There are several things to consider when using the ENI trunking feature\.
+ Only Linux variants of the Amazon ECS\-optimized AMI, or other Amazon Linux variants with the `ecs-init` package, support the increased ENI limits\. Windows containers are not supported at this time\.
+ Only new Amazon EC2 instances launched after opting in to `awsvpcTrunking` receive the increased ENI limits and the trunk network interface\. Previously launched instances do not receive these features regardless of the actions taken\.
+ Your Amazon ECS tasks must use the `awsvpc` network mode and the EC2 launch type\. Tasks using the Fargate launch type always received a dedicated ENI regardless of how many are launched, so this feature is not needed\.
+ When launching a new container instance, the instance transitions to a `REGISTERING` status while the trunk elastic network interface is provisioned for the instance\. If the registration fails, the instance transitions to a `REGISTRATION_FAILED` status\. You can describe the container instance and see the reason for failure in the `statusReason` parameter\.
+ Once the container instance is terminated, the instance transitions to a `DEREGISTERING` status while the trunk elastic network interface is deprovisioned\. The instance then transitions to an `INACTIVE` status\.
+ If a container instance in a public subnet with the increased ENI limits is stopped and then restarted, the instance loses its public IP address, and the container agent loses its connection\.

## Working With Container Instances With Increased ENI Limits<a name="eni-trunking-launching"></a>

Before you launch a container instance with the increased ENI limits, the following prerequisites must be completed\.
+ The service\-linked role for Amazon ECS must be created\. The Amazon ECS service\-linked role provides Amazon ECS with the permissions to make calls to other AWS services on your behalf\. This role is created for you automatically when you create a cluster, or if you create or update a service in the AWS Management Console\. For more information, see [Using Service\-Linked Roles for Amazon ECS](using-service-linked-roles.md)\. You can also create the service\-linked role with the following AWS CLI command\.

  ```
  aws iam [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) --aws-service-name ecs.amazonaws.com
  ```
+ Your account, IAM user, or role must be opted in to the `awsvpcTrunking` account setting\. For more information, see [Account Settings](ecs-account-settings.md)\. 
+ The container instance must be one of the supported Amazon EC2 instance types, launched after opting in to the `awsvpcTrunking` account setting, and requires version 1\.28\.1 or later of the container agent\. For a list of supported instance types, see [Supported Amazon EC2 Instance Types](#eni-trunking-supported-instance-types)\.

Once the prerequisites are met, you can launch a new container instance using one of the supported Amazon EC2 instance types, and the instance will have the increased ENI limits\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

**To view your container instances with increased ENI limits with the AWS CLI**

Each container instance has a default network interface, referred to as a trunk network interface\. Use the following command to list your container instances with increased ENI limits by querying for the `ecs.awsvpc-trunk-id` attribute, which indicates it has a trunk network interface\.
+ [list\-attributes](https://docs.aws.amazon.com/cli/latest/reference/ecs/list-attributes.html) \(AWS CLI\)

  ```
  aws ecs list-attributes --target-type container-instance --attribute-name ecs.awsvpc-trunk-id --region us-east-1
  ```
+ [Get\-ECSAttributeList](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECSAttributeList.html) \(AWS Tools for Windows PowerShell\)

  ```
  Get-ECSAttributeList -TargetType container-instance -AttributeName ecs.awsvpc-trunk-id -Region us-east-1
  ```

## Supported Amazon EC2 Instance Types<a name="eni-trunking-supported-instance-types"></a>

The following Amazon EC2 instance types are supported when using the increased ENI trunking feature\.


| Instance Type | Current Network Interface Limit | New Network Interface Limit | 
| --- | --- | --- | 
| c5 instance family | 
|  c5\.large  | 3 | 10 | 
|  c5\.xlarge  | 4 | 20 | 
|  c5\.2xlarge  | 4 | 40 | 
|  c5\.4xlarge  | 8 | 60 | 
|  c5\.9xlarge  | 8 | 60 | 
|  c5\.18xlarge  | 15 | 120 | 
| m5 instance family | 
|  m5\.large  | 3 | 10 | 
|  m5\.xlarge  | 4 | 20 | 
|  m5\.2xlarge  | 4 | 40 | 
|  m5\.4xlarge  | 8 | 60 | 
|  m5\.12xlarge  | 8 | 60 | 
|  m5\.24xlarge  | 15 | 120 | 
| p3 instance family | 
|  p3\.2xlarge  | 4 | 40 | 
|  p3\.8xlarge  | 8 | 60 | 
|  p3\.16xlarge  | 8 | 120 | 
|  p3dn\.24xlarge  | 15 | 120 | 
| r5 instance family | 
|  r5\.large  | 3 | 10 | 
|  r5\.xlarge  | 4 | 20 | 
|  r5\.2xlarge  | 4 | 40 | 
|  r5\.4xlarge  | 8 | 60 | 
|  r5\.12xlarge  | 8 | 60 | 
|  r5\.24xlarge  | 15 | 120 | 