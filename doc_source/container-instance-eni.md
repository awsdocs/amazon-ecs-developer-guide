# Elastic network interface trunking<a name="container-instance-eni"></a>

Each Amazon ECS task that uses the `awsvpc` network mode receives its own elastic network interface \(ENI\), which is attached to the container instance that hosts it\. There is a default limit to the number of network interfaces that can be attached to an Amazon EC2 instance, and the primary network interface counts as one\. For example, by default a `c5.large` instance may have up to three ENIs attached to it\. The primary network interface for the instance counts as one, so you can attach an additional two ENIs to the instance\. Because each task using the `awsvpc` network mode requires an ENI, you can typically only run two such tasks on this instance type\.

Amazon ECS supports launching container instances with increased ENI density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. This configuration allows you to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI limit of twelve\. The container instance will have the primary network interface and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. So this configuration allows you to launch ten tasks on the container instance instead of the current two tasks\.

The trunk network interface is fully managed by Amazon ECS and is deleted when you either terminate or deregister your container instance from the cluster\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

## ENI trunking considerations<a name="eni-trunking-considerations"></a>

There are several things to consider when using the ENI trunking feature\.
+ Only Linux variants of the Amazon ECS\-optimized AMI, or other Amazon Linux variants with version `1.28.1` or later of the container agent and version `1.28.1-2` or later of the ecs\-init package, support the increased ENI limits\. If you use the latest Linux variant of the Amazon ECS\-optimized AMI, these requirements will be met\. Windows containers are not supported at this time\.
+ Only new Amazon EC2 instances launched after opting in to `awsvpcTrunking` receive the increased ENI limits and the trunk network interface\. Previously launched instances do not receive these features regardless of the actions taken\.
+ Amazon EC2 instances in shared subnets are not supported\. They will fail to register to a cluster if they are used\.
+ Your Amazon ECS tasks must use the `awsvpc` network mode and the EC2 launch type\. Tasks using the Fargate launch type always received a dedicated ENI regardless of how many are launched, so this feature is not needed\.
+ Your Amazon ECS tasks must be launched in the same Amazon VPC as your container instance\. Your tasks will fail to start with an attribute error if they are not within the same VPC\.
+ When launching a new container instance, the instance transitions to a `REGISTERING` status while the trunk elastic network interface is provisioned for the instance\. If the registration fails, the instance transitions to a `REGISTRATION_FAILED` status\. You can describe the container instance and see the reason for failure in the `statusReason` parameter\.
+ Once the container instance is terminated, the instance transitions to a `DEREGISTERING` status while the trunk elastic network interface is deprovisioned\. The instance then transitions to an `INACTIVE` status\.
+ If a container instance in a public subnet with the increased ENI limits is stopped and then restarted, the instance loses its public IP address, and the container agent loses its connection\.

## Working With container instances with increased ENI limits<a name="eni-trunking-launching"></a>

Before you launch a container instance with the increased ENI limits, the following prerequisites must be completed\.
+ The service\-linked role for Amazon ECS must be created\. The Amazon ECS service\-linked role provides Amazon ECS with the permissions to make calls to other AWS services on your behalf\. This role is created for you automatically when you create a cluster, or if you create or update a service in the AWS Management Console\. For more information, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\. You can also create the service\-linked role with the following AWS CLI command\.

  ```
  aws iam [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) --aws-service-name ecs.amazonaws.com
  ```
+ Your account or container instance IAM role must opt\-in to the `awsvpcTrunking` account setting\. This can be done in the following ways:
  + Any user can use the PutAccountSettingDefault API to opt\-in all IAM users and roles on an account
  + A root user can use the PutAccountSetting API to opt\-in the IAM user or container instance role that will register the instance with the cluster
  + A container instance role can opt itself in when the PutAccountSetting API is run on an instance prior to it being registered with a cluster

  For more information, see [Account settings](ecs-account-settings.md)\.

Once the prerequisites are met, you can launch a new container instance using one of the supported Amazon EC2 instance types, and the instance will have the increased ENI limits\. For a list of supported instance types, see [Supported Amazon EC2 instance types](#eni-trunking-supported-instance-types)\. The container instance must have version `1.28.1` or later of the container agent and version `1.28.1-2` or later of the ecs\-init package\. If you use the latest Linux variant of the Amazon ECS\-optimized AMI, these requirements will be met\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

**To opt in all IAM users or roles on your account to the increased ENI limits using the console**

1. As the root user of the account, open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to opt in to the increased ENI limits\.

1. From the dashboard, choose **Account Settings**\.

1. For **IAM user or role**, ensure your root user or container instance IAM role is selected\.

1. For **AWSVPC Trunking**, select the check box\. Choose **Save** once finished\.
**Important**  
IAM users and IAM roles need the `ecs:PutAccountSetting` permission to perform this action\.

1. On the confirmation screen, choose **Confirm** to save the selection\.

**To opt in all IAM users or roles on your account to the increased ENI limits using the command line**

Any user on an account can use one of the following commands to modify the default account setting for all IAM users or roles on your account\. These changes apply to the entire AWS account unless an IAM user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting-default \
        --name awsvpcTrunking \
        --value enabled \
        --region us-east-1
  ```
+ [Write\-ECSAccountSettingDefault](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSettingDefault.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSettingDefault -Name awsvpcTrunking -Value enabled -Region us-east-1 -Force
  ```

**To opt in an IAM user or container instance IAM role to the increased ENI limits as the root user using the command line**

The root user on an account can use one of the following commands and specify the ARN of the principal IAM user or container instance IAM role in the request to modify the account settings\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  The following example is for modifying the account setting of a specific IAM user:

  ```
  aws ecs put-account-setting \
        --name awsvpcTrunking \
        --value enabled \
        --principal-arn arn:aws:iam::aws_account_id:user/userName \
        --region us-east-1
  ```

  The following example is for modifying the account setting of a specific container instance IAM role:

  ```
  aws ecs put-account-setting \
        --name awsvpcTrunking \
        --value enabled \
        --principal-arn arn:aws:iam::aws_account_id:role/ecsInstanceRole \
        --region us-east-1
  ```
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  The following example is for modifying the account setting of a specific IAM user:

  ```
  Write-ECSAccountSetting -Name awsvpcTrunking -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/userName -Region us-east-1 -Force
  ```

  The following example is for modifying the account setting of a specific container instance IAM role:

  ```
  Write-ECSAccountSetting -Name awsvpcTrunking -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:role/ecsInstanceRole -Region us-east-1 -Force
  ```

**To view your container instances with increased ENI limits with the AWS CLI**

Each container instance has a default network interface, referred to as a trunk network interface\. Use the following command to list your container instances with increased ENI limits by querying for the `ecs.awsvpc-trunk-id` attribute, which indicates it has a trunk network interface\.
+ [list\-attributes](https://docs.aws.amazon.com/cli/latest/reference/ecs/list-attributes.html) \(AWS CLI\)

  ```
  aws ecs list-attributes \
        --target-type container-instance \
        --attribute-name ecs.awsvpc-trunk-id \
        --cluster cluster_name \
        --region us-east-1
  ```
+ [Get\-ECSAttributeList](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECSAttributeList.html) \(AWS Tools for Windows PowerShell\)

  ```
  Get-ECSAttributeList -TargetType container-instance -AttributeName ecs.awsvpc-trunk-id -Region us-east-1
  ```

## Supported Amazon EC2 instance types<a name="eni-trunking-supported-instance-types"></a>

The following shows the supported Amazon EC2 instance types and how many tasks using the `awsvpc` network mode can be launched on each instance type before and after opting in to the `awsvpcTrunking` account setting\. For the elastic network interface \(ENI\) limits on each instance type, add one to the current task limit, as the primary network interface counts against the limit, and add two to the new task limit, as both the primary network interface and the trunk network instance count again the limit\.

**Important**  
Although other instance types are supported in the same instance family, the `c5n`, `m5n`, `m5dn`, `r5n`, and `r5dn` instance types are not supported\.


| Instance Type | Current Task Limit per Instance | New Task Limit per Instance | 
| --- |--- |--- |
| a1 instance family | 
| --- |
|  a1\.medium  | 1 | 10 | 
|  a1\.large  | 2 | 10 | 
|  a1\.xlarge  | 3 | 20 | 
|  a1\.2xlarge  | 3 | 40 | 
|  a1\.4xlarge  | 7 | 60 | 
| c5 instance family | 
| --- |
|  c5\.large  | 2 | 10 | 
|  c5\.xlarge  | 3 | 20 | 
|  c5\.2xlarge  | 3 | 40 | 
|  c5\.4xlarge  | 7 | 60 | 
|  c5\.9xlarge  | 7 | 60 | 
|  c5\.18xlarge  | 14 | 120 | 
|  c5d\.large  | 2 | 10 | 
|  c5d\.xlarge  | 3 | 20 | 
|  c5d\.2xlarge  | 3 | 40 | 
|  c5d\.4xlarge  | 7 | 60 | 
|  c5d\.9xlarge  | 7 | 60 | 
|  c5d\.18xlarge  | 14 | 120 | 
| m5 instance family | 
| --- |
|  m5\.large  | 2 | 10 | 
|  m5\.xlarge  | 3 | 20 | 
|  m5\.2xlarge  | 3 | 40 | 
|  m5\.4xlarge  | 7 | 60 | 
|  m5\.12xlarge  | 7 | 60 | 
|  m5\.24xlarge  | 14 | 120 | 
|  m5a\.large  | 2 | 10 | 
|  m5a\.xlarge  | 3 | 20 | 
|  m5a\.2xlarge  | 3 | 40 | 
|  m5a\.4xlarge  | 7 | 60 | 
|  m5a\.12xlarge  | 7 | 60 | 
|  m5a\.24xlarge  | 14 | 120 | 
|  m5ad\.large  | 2 | 10 | 
|  m5ad\.xlarge  | 3 | 20 | 
|  m5ad\.2xlarge  | 3 | 40 | 
|  m5ad\.4xlarge  | 7 | 60 | 
|  m5ad\.12xlarge  | 7 | 60 | 
|  m5ad\.24xlarge  | 14 | 120 | 
|  m5d\.large  | 2 | 10 | 
|  m5d\.xlarge  | 3 | 20 | 
|  m5d\.2xlarge  | 3 | 40 | 
|  m5d\.4xlarge  | 7 | 60 | 
|  m5d\.12xlarge  | 7 | 60 | 
|  m5d\.24xlarge  | 14 | 120 | 
| p3 instance family | 
| --- |
|  p3\.2xlarge  | 3 | 40 | 
|  p3\.8xlarge  | 7 | 60 | 
|  p3\.16xlarge  | 7 | 120 | 
| r5 instance family | 
| --- |
|  r5\.large  | 2 | 10 | 
|  r5\.xlarge  | 3 | 20 | 
|  r5\.2xlarge  | 3 | 40 | 
|  r5\.4xlarge  | 7 | 60 | 
|  r5\.12xlarge  | 7 | 60 | 
|  r5\.24xlarge  | 14 | 120 | 
|  r5a\.large  | 2 | 10 | 
|  r5a\.xlarge  | 3 | 20 | 
|  r5a\.2xlarge  | 3 | 40 | 
|  r5a\.4xlarge  | 7 | 60 | 
|  r5a\.12xlarge  | 7 | 60 | 
|  r5a\.24xlarge  | 14 | 120 | 
|  r5ad\.large  | 2 | 10 | 
|  r5ad\.xlarge  | 3 | 20 | 
|  r5ad\.2xlarge  | 3 | 40 | 
|  r5ad\.4xlarge  | 7 | 60 | 
|  r5ad\.12xlarge  | 7 | 60 | 
|  r5ad\.24xlarge  | 14 | 120 | 
|  r5d\.large  | 2 | 10 | 
|  r5d\.xlarge  | 3 | 20 | 
|  r5d\.2xlarge  | 3 | 40 | 
|  r5d\.4xlarge  | 7 | 60 | 
|  r5d\.12xlarge  | 7 | 60 | 
|  r5d\.24xlarge  | 14 | 120 | 