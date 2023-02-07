# Elastic network interface trunking<a name="container-instance-eni"></a>

**Note**  
This feature is not available on Fargate\.

Each Amazon ECS task that uses the `awsvpc` network mode receives its own elastic network interface \(ENI\), which is attached to the container instance that hosts it\. There is a default limit to the number of network interfaces that can be attached to an Amazon EC2 instance, and the primary network interface counts as one\. For example, by default a `c5.large` instance may have up to three ENIs attached to it\. The primary network interface for the instance counts as one, so you can attach an additional two ENIs to the instance\. Because each task using the `awsvpc` network mode requires an ENI, you can typically only run two such tasks on this instance type\.

Amazon ECS supports launching container instances with increased ENI density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. This configuration allows you to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI limit of twelve\. The container instance will have the primary network interface and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. So this configuration allows you to launch ten tasks on the container instance instead of the current two tasks\.

The trunk network interface is fully managed by Amazon ECS and is deleted when you either terminate or deregister your container instance from the cluster\. For more information, see [Amazon ECS task networking](task-networking.md)\.

## ENI trunking considerations<a name="eni-trunking-considerations"></a>

There are several things to consider when using the ENI trunking feature\.
+ Only Linux variants of the Amazon ECS\-optimized AMI, or other Amazon Linux variants with version `1.28.1` or later of the container agent and version `1.28.1-2` or later of the ecs\-init package, support the increased ENI limits\. If you use the latest Linux variant of the Amazon ECS\-optimized AMI, these requirements will be met\. Windows containers are not supported at this time\.
+ Only new Amazon EC2 instances launched after opting in to `awsvpcTrunking` receive the increased ENI limits and the trunk network interface\. Previously launched instances do not receive these features regardless of the actions taken\.
+ Amazon EC2 instances must have resource\-based IPv4 DNS requests disabled\. To disable this option, ensure the **Enable resource\-based IPV4 \(A record\) DNS requests** option is deselected when creating a new instance using the Amazon EC2 console\. To disable this option using the AWS CLI, use the following command\.

  ```
  aws ec2 modify-private-dns-name-options --instance-id i-xxxxxxx --no-enable-resource-name-dns-a-record --no-dry-run
  ```
+ Amazon EC2 instances in shared subnets are not supported\. They will fail to register to a cluster if they are used\.
+ Your Amazon ECS tasks must use the `awsvpc` network mode and the EC2 launch type\. Tasks using the Fargate launch type always received a dedicated ENI regardless of how many are launched, so this feature is not needed\.
+ Your Amazon ECS tasks must be launched in the same Amazon VPC as your container instance\. Your tasks will fail to start with an attribute error if they are not within the same VPC\.
+ When launching a new container instance, the instance transitions to a `REGISTERING` status while the trunk elastic network interface is provisioned for the instance\. If the registration fails, the instance transitions to a `REGISTRATION_FAILED` status\. You can troubleshoot a failed registration by describing the container instance to view the `statusReason` field which describes the reason for the failure\. The container instance then can be manually deregistered or terminated\. Once the container instance is successfully deregistered or terminated, Amazon ECS will delete the trunk ENI\.
**Note**  
Amazon ECS emits container instance state change events which you can monitor for instances that transition to a `REGISTRATION_FAILED` state\. For more information, see [Container instance state change events](ecs_cwe_events.md#ecs_container_instance_events)\.
+ Once the container instance is terminated, the instance transitions to a `DEREGISTERING` status while the trunk elastic network interface is deprovisioned\. The instance then transitions to an `INACTIVE` status\.
+ If a container instance in a public subnet with the increased ENI limits is stopped and then restarted, the instance loses its public IP address, and the container agent loses its connection\.

## Working with container instances with increased ENI limits<a name="eni-trunking-launching"></a>

Before you launch a container instance with the increased ENI limits, the following prerequisites must be completed\.
+ The service\-linked role for Amazon ECS must be created\. The Amazon ECS service\-linked role provides Amazon ECS with the permissions to make calls to other AWS services on your behalf\. This role is created for you automatically when you create a cluster, or if you create or update a service in the AWS Management Console\. For more information, see [Using service\-linked roles for Amazon ECS](using-service-linked-roles.md)\. You can also create the service\-linked role with the following AWS CLI command\.

  ```
  aws iam [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) --aws-service-name ecs.amazonaws.com
  ```
+ Your account or container instance IAM role must opt\-in to the `awsvpcTrunking` account setting\. This can be done in the following ways:
  + Any user can use the PutAccountSettingDefault API to opt\-in all IAM users and roles on an account
  + A root user can use the PutAccountSetting API to opt\-in the user or container instance role that will register the instance with the cluster
  + A container instance role can opt itself in when the PutAccountSetting API is run on an instance prior to it being registered with a cluster

  For more information, see [Account settings](ecs-account-settings.md)\.

Once the prerequisites are met, you can launch a new container instance using one of the supported Amazon EC2 instance types, and the instance will have the increased ENI limits\. For a list of supported instance types, see [Supported Amazon EC2 instance types](#eni-trunking-supported-instance-types)\. The container instance must have version `1.28.1` or later of the container agent and version `1.28.1-2` or later of the ecs\-init package\. If you use the latest Linux variant of the Amazon ECS\-optimized AMI, these requirements will be met\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

**Important**  
Amazon EC2 instances must have resource\-based IPv4 DNS requests disabled\. To disable this option, ensure the **Enable resource\-based IPV4 \(A record\) DNS requests** option is deselected when creating a new instance using the Amazon EC2 console\. To disable this option using the AWS CLI, use the following command\.  

```
aws ec2 modify-private-dns-name-options --instance-id i-xxxxxxx --no-enable-resource-name-dns-a-record --no-dry-run
```

**To opt in all IAM users or roles on your account to the increased ENI limits \(AWS Management Console\)**

1. As the account owner, open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to opt in to the increased ENI limits\.

1. From the dashboard, choose **Account Settings**\.

1. For **IAM user or role**, ensure your root user or container instance IAM role is selected\.

1. For **AWSVPC Trunking**, select the check box\. Choose **Save** once finished\.
**Important**  
IAM users and IAM roles need the `ecs:PutAccountSetting` permission to perform this action\.

1. On the confirmation screen, choose **Confirm** to save the selection\.

**To opt in all user or roles on your account to the increased ENI limits using the command line**

Any user on an account can use one of the following commands to modify the default account setting for all IAM users or roles on your account\. These changes apply to the entire AWS account unless a user or role explicitly overrides these settings for themselves\.
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

**To opt in a user or container instance IAM role to the increased ENI limits as the account owner using the command line**

The account owner can use one of the following commands and specify the ARN of the principal user or container instance IAM role in the request to modify the account settings\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  The following example is for modifying the account setting of a specific user:

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

  The following example is for modifying the account setting of a specific user:

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
Although other instance types are supported in the same instance family, the `a1.metal`, `c5.metal`, `c5a.8xlarge`, `c5ad.8xlarge`, `c5d.metal`, `m5.metal`, `p3dn.24xlarge`, `r5.metal`, `r5.8xlarge`, and `r5d.metal` instance types are not supported\.  
The `c5n`, `d3`, `d3en`, `g3`, `g3s`, `g4dn`, `i3`, `i3en`, `inf1`, `m5dn`, `m5n`, `m5zn`, `mac1`, `r5b`, `r5n`, `r5dn`, `u-12tb1`, `u-6tb1`, `u-9tb1`, and `z1d` instance families are not supported\.

**Topics**
+ [General purpose](#eni-branch-gp)
+ [Compute optimized](#eni-branch-co)
+ [Memory optimized](#eni-branch-mo)
+ [Storage optimized](#eni-branch-so)
+ [Accelerated computing](#eni-branch-ac)

### General purpose<a name="eni-branch-gp"></a>


| Instance type | Task limit without ENI trunking | Task limit with ENI trunking | 
| --- | --- | --- | 
| a1\.medium | 1 | 10 | 
| a1\.large | 2 | 10 | 
| a1\.xlarge | 3 | 20 | 
| a1\.2xlarge | 3 | 40 | 
| a1\.4xlarge | 7 | 60 | 
| m5\.large | 2 | 10 | 
| m5\.xlarge | 3 | 20 | 
| m5\.2xlarge | 3 | 40 | 
| m5\.4xlarge | 7 | 60 | 
| m5\.8xlarge | 7 | 60 | 
| m5\.12xlarge | 7 | 60 | 
| m5\.16xlarge | 14 | 120 | 
| m5\.24xlarge | 14 | 120 | 
| m5a\.large | 2 | 10 | 
| m5a\.xlarge | 3 | 20 | 
| m5a\.2xlarge | 3 | 40 | 
| m5a\.4xlarge | 7 | 60 | 
| m5a\.8xlarge | 7 | 60 | 
| m5a\.12xlarge | 7 | 60 | 
| m5a\.16xlarge | 14 | 120 | 
| m5a\.24xlarge | 14 | 120 | 
| m5ad\.large | 2 | 10 | 
| m5ad\.xlarge | 3 | 20 | 
| m5ad\.2xlarge | 3 | 40 | 
| m5ad\.4xlarge | 7 | 60 | 
| m5ad\.8xlarge | 7 | 60 | 
| m5ad\.12xlarge | 7 | 60 | 
| m5ad\.16xlarge | 14 | 120 | 
| m5ad\.24xlarge | 14 | 120 | 
| m5d\.large | 2 | 10 | 
| m5d\.xlarge | 3 | 20 | 
| m5d\.2xlarge | 3 | 40 | 
| m5d\.4xlarge | 7 | 60 | 
| m5d\.8xlarge | 7 | 60 | 
| m5d\.12xlarge | 7 | 60 | 
| m5d\.16xlarge | 14 | 120 | 
| m5d\.24xlarge | 14 | 120 | 
| m5d\.metal | 14 | 120 | 
| m6a\.large | 2 | 10 | 
| m6a\.xlarge | 3 | 20 | 
| m6a\.2xlarge | 3 | 40 | 
| m6a\.4xlarge | 7 | 60 | 
| m6a\.8xlarge | 7 | 90 | 
| m6a\.12xlarge | 7 | 120 | 
| m6a\.16xlarge | 14 | 120 | 
| m6a\.24xlarge | 14 | 120 | 
| m6a\.32xlarge | 14 | 120 | 
| m6a\.48xlarge | 14 | 120 | 
| m6a\.metal | 14 | 120 | 
| m6g\.medium | 1 | 4 | 
| m6g\.large | 2 | 10 | 
| m6g\.xlarge | 3 | 20 | 
| m6g\.2xlarge | 3 | 40 | 
| m6g\.4xlarge | 7 | 60 | 
| m6g\.8xlarge | 7 | 60 | 
| m6g\.12xlarge | 7 | 60 | 
| m6g\.16xlarge | 14 | 120 | 
| m6g\.metal | 14 | 120 | 
| m6gd\.medium | 1 | 4 | 
| m6gd\.large | 2 | 10 | 
| m6gd\.xlarge | 3 | 20 | 
| m6gd\.2xlarge | 3 | 40 | 
| m6gd\.4xlarge | 7 | 60 | 
| m6gd\.8xlarge | 7 | 60 | 
| m6gd\.12xlarge | 7 | 60 | 
| m6gd\.16xlarge | 14 | 120 | 
| m6gd\.metal | 14 | 120 | 
| m6i\.large | 2 | 10 | 
| m6i\.xlarge | 3 | 20 | 
| m6i\.2xlarge | 3 | 40 | 
| m6i\.4xlarge | 7 | 60 | 
| m6i\.8xlarge | 7 | 90 | 
| m6i\.12xlarge | 7 | 120 | 
| m6i\.16xlarge | 14 | 120 | 
| m6i\.24xlarge | 14 | 120 | 
| m6i\.32xlarge | 14 | 120 | 
| m6i\.metal | 14 | 120 | 
| m6id\.large | 2 | 10 | 
| m6id\.xlarge | 3 | 20 | 
| m6id\.2xlarge | 3 | 40 | 
| m6id\.4xlarge | 7 | 60 | 
| m6id\.8xlarge | 7 | 90 | 
| m6id\.12xlarge | 7 | 120 | 
| m6id\.16xlarge | 14 | 120 | 
| m6id\.24xlarge | 14 | 120 | 
| m6id\.32xlarge | 14 | 120 | 
| m6id\.metal | 14 | 120 | 
| m6idn\.large | 2 | 10 | 
| m6idn\.xlarge | 3 | 20 | 
| m6idn\.2xlarge | 3 | 40 | 
| m6idn\.4xlarge | 7 | 60 | 
| m6idn\.8xlarge | 7 | 90 | 
| m6idn\.12xlarge | 7 | 120 | 
| m6idn\.16xlarge | 14 | 120 | 
| m6idn\.24xlarge | 14 | 120 | 
| m6idn\.32xlarge | 13 | 120 | 
| m6in\.large | 2 | 10 | 
| m6in\.xlarge | 3 | 20 | 
| m6in\.2xlarge | 3 | 40 | 
| m6in\.4xlarge | 7 | 60 | 
| m6in\.8xlarge | 7 | 90 | 
| m6in\.12xlarge | 7 | 120 | 
| m6in\.16xlarge | 14 | 120 | 
| m6in\.24xlarge | 14 | 120 | 
| m6in\.32xlarge | 13 | 120 | 
| mac2\.metal | 7 | 12 | 

### Compute optimized<a name="eni-branch-co"></a>


| Instance type | Task limit without ENI trunking | Task limit with ENI trunking | 
| --- | --- | --- | 
| c5\.large | 2 | 10 | 
| c5\.xlarge | 3 | 20 | 
| c5\.2xlarge | 3 | 40 | 
| c5\.4xlarge | 7 | 60 | 
| c5\.9xlarge | 7 | 60 | 
| c5\.12xlarge | 7 | 60 | 
| c5\.18xlarge | 14 | 120 | 
| c5\.24xlarge | 14 | 120 | 
| c5a\.large | 2 | 10 | 
| c5a\.xlarge | 3 | 20 | 
| c5a\.2xlarge | 3 | 40 | 
| c5a\.4xlarge | 7 | 60 | 
| c5a\.12xlarge | 7 | 60 | 
| c5a\.16xlarge | 14 | 120 | 
| c5a\.24xlarge | 14 | 120 | 
| c5ad\.large | 2 | 10 | 
| c5ad\.xlarge | 3 | 20 | 
| c5ad\.2xlarge | 3 | 40 | 
| c5ad\.4xlarge | 7 | 60 | 
| c5ad\.12xlarge | 7 | 60 | 
| c5ad\.16xlarge | 14 | 120 | 
| c5ad\.24xlarge | 14 | 120 | 
| c5d\.large | 2 | 10 | 
| c5d\.xlarge | 3 | 20 | 
| c5d\.2xlarge | 3 | 40 | 
| c5d\.4xlarge | 7 | 60 | 
| c5d\.9xlarge | 7 | 60 | 
| c5d\.12xlarge | 7 | 60 | 
| c5d\.18xlarge | 14 | 120 | 
| c5d\.24xlarge | 14 | 120 | 
| c6a\.large | 2 | 10 | 
| c6a\.xlarge | 3 | 20 | 
| c6a\.2xlarge | 3 | 40 | 
| c6a\.4xlarge | 7 | 60 | 
| c6a\.8xlarge | 7 | 90 | 
| c6a\.12xlarge | 7 | 120 | 
| c6a\.16xlarge | 14 | 120 | 
| c6a\.24xlarge | 14 | 120 | 
| c6a\.32xlarge | 14 | 120 | 
| c6a\.48xlarge | 14 | 120 | 
| c6a\.metal | 14 | 120 | 
| c6g\.medium | 1 | 4 | 
| c6g\.large | 2 | 10 | 
| c6g\.xlarge | 3 | 20 | 
| c6g\.2xlarge | 3 | 40 | 
| c6g\.4xlarge | 7 | 60 | 
| c6g\.8xlarge | 7 | 60 | 
| c6g\.12xlarge | 7 | 60 | 
| c6g\.16xlarge | 14 | 120 | 
| c6g\.metal | 14 | 120 | 
| c6gd\.medium | 1 | 4 | 
| c6gd\.large | 2 | 10 | 
| c6gd\.xlarge | 3 | 20 | 
| c6gd\.2xlarge | 3 | 40 | 
| c6gd\.4xlarge | 7 | 60 | 
| c6gd\.8xlarge | 7 | 60 | 
| c6gd\.12xlarge | 7 | 60 | 
| c6gd\.16xlarge | 14 | 120 | 
| c6gd\.metal | 14 | 120 | 
| c6gn\.medium | 1 | 4 | 
| c6gn\.large | 2 | 10 | 
| c6gn\.xlarge | 3 | 20 | 
| c6gn\.2xlarge | 3 | 40 | 
| c6gn\.4xlarge | 7 | 60 | 
| c6gn\.8xlarge | 7 | 60 | 
| c6gn\.12xlarge | 7 | 60 | 
| c6gn\.16xlarge | 14 | 120 | 
| c6i\.large | 2 | 10 | 
| c6i\.xlarge | 3 | 20 | 
| c6i\.2xlarge | 3 | 40 | 
| c6i\.4xlarge | 7 | 60 | 
| c6i\.8xlarge | 7 | 90 | 
| c6i\.12xlarge | 7 | 120 | 
| c6i\.16xlarge | 14 | 120 | 
| c6i\.24xlarge | 14 | 120 | 
| c6i\.32xlarge | 14 | 120 | 
| c6i\.metal | 14 | 120 | 
| c6id\.large | 2 | 10 | 
| c6id\.xlarge | 3 | 20 | 
| c6id\.2xlarge | 3 | 40 | 
| c6id\.4xlarge | 7 | 60 | 
| c6id\.8xlarge | 7 | 90 | 
| c6id\.12xlarge | 7 | 120 | 
| c6id\.16xlarge | 14 | 120 | 
| c6id\.24xlarge | 14 | 120 | 
| c6id\.32xlarge | 14 | 120 | 
| c6id\.metal | 14 | 120 | 
| c6in\.large | 2 | 10 | 
| c6in\.xlarge | 3 | 20 | 
| c6in\.2xlarge | 3 | 40 | 
| c6in\.4xlarge | 7 | 60 | 
| c6in\.8xlarge | 7 | 90 | 
| c6in\.12xlarge | 7 | 120 | 
| c6in\.16xlarge | 14 | 120 | 
| c6in\.24xlarge | 14 | 120 | 
| c6in\.32xlarge | 13 | 120 | 
| c7g\.medium | 1 | 4 | 
| c7g\.large | 2 | 10 | 
| c7g\.xlarge | 3 | 20 | 
| c7g\.2xlarge | 3 | 40 | 
| c7g\.4xlarge | 7 | 60 | 
| c7g\.8xlarge | 7 | 60 | 
| c7g\.12xlarge | 7 | 60 | 
| c7g\.16xlarge | 14 | 120 | 
| hpc6a\.48xlarge | 1 | 120 | 

### Memory optimized<a name="eni-branch-mo"></a>


| Instance type | Task limit without ENI trunking | Task limit with ENI trunking | 
| --- | --- | --- | 
| hpc6id\.32xlarge | 1 | 120 | 
| r5\.large | 2 | 10 | 
| r5\.xlarge | 3 | 20 | 
| r5\.2xlarge | 3 | 40 | 
| r5\.4xlarge | 7 | 60 | 
| r5\.12xlarge | 7 | 60 | 
| r5\.16xlarge | 14 | 120 | 
| r5\.24xlarge | 14 | 120 | 
| r5a\.large | 2 | 10 | 
| r5a\.xlarge | 3 | 20 | 
| r5a\.2xlarge | 3 | 40 | 
| r5a\.4xlarge | 7 | 60 | 
| r5a\.8xlarge | 7 | 60 | 
| r5a\.12xlarge | 7 | 60 | 
| r5a\.16xlarge | 14 | 120 | 
| r5a\.24xlarge | 14 | 120 | 
| r5ad\.large | 2 | 10 | 
| r5ad\.xlarge | 3 | 20 | 
| r5ad\.2xlarge | 3 | 40 | 
| r5ad\.4xlarge | 7 | 60 | 
| r5ad\.8xlarge | 7 | 60 | 
| r5ad\.12xlarge | 7 | 60 | 
| r5ad\.16xlarge | 14 | 120 | 
| r5ad\.24xlarge | 14 | 120 | 
| r5d\.large | 2 | 10 | 
| r5d\.xlarge | 3 | 20 | 
| r5d\.2xlarge | 3 | 40 | 
| r5d\.4xlarge | 7 | 60 | 
| r5d\.8xlarge | 7 | 60 | 
| r5d\.12xlarge | 7 | 60 | 
| r5d\.16xlarge | 14 | 120 | 
| r5d\.24xlarge | 14 | 120 | 
| r6a\.large | 2 | 10 | 
| r6a\.xlarge | 3 | 20 | 
| r6a\.2xlarge | 3 | 40 | 
| r6a\.4xlarge | 7 | 60 | 
| r6a\.8xlarge | 7 | 90 | 
| r6a\.12xlarge | 7 | 120 | 
| r6a\.16xlarge | 14 | 120 | 
| r6a\.24xlarge | 14 | 120 | 
| r6a\.32xlarge | 14 | 120 | 
| r6a\.48xlarge | 14 | 120 | 
| r6a\.metal | 14 | 120 | 
| r6g\.medium | 1 | 4 | 
| r6g\.large | 2 | 10 | 
| r6g\.xlarge | 3 | 20 | 
| r6g\.2xlarge | 3 | 40 | 
| r6g\.4xlarge | 7 | 60 | 
| r6g\.8xlarge | 7 | 60 | 
| r6g\.12xlarge | 7 | 60 | 
| r6g\.16xlarge | 14 | 120 | 
| r6g\.metal | 14 | 120 | 
| r6gd\.medium | 1 | 4 | 
| r6gd\.large | 2 | 10 | 
| r6gd\.xlarge | 3 | 20 | 
| r6gd\.2xlarge | 3 | 40 | 
| r6gd\.4xlarge | 7 | 60 | 
| r6gd\.8xlarge | 7 | 60 | 
| r6gd\.12xlarge | 7 | 60 | 
| r6gd\.16xlarge | 14 | 120 | 
| r6gd\.metal | 14 | 120 | 
| r6i\.large | 2 | 10 | 
| r6i\.xlarge | 3 | 20 | 
| r6i\.2xlarge | 3 | 40 | 
| r6i\.4xlarge | 7 | 60 | 
| r6i\.8xlarge | 7 | 90 | 
| r6i\.12xlarge | 7 | 120 | 
| r6i\.16xlarge | 14 | 120 | 
| r6i\.24xlarge | 14 | 120 | 
| r6i\.32xlarge | 14 | 120 | 
| r6i\.metal | 14 | 120 | 
| r6idn\.large | 2 | 10 | 
| r6idn\.xlarge | 3 | 20 | 
| r6idn\.2xlarge | 3 | 40 | 
| r6idn\.4xlarge | 7 | 60 | 
| r6idn\.8xlarge | 7 | 90 | 
| r6idn\.12xlarge | 7 | 120 | 
| r6idn\.16xlarge | 14 | 120 | 
| r6idn\.24xlarge | 14 | 120 | 
| r6idn\.32xlarge | 13 | 120 | 
| r6in\.large | 2 | 10 | 
| r6in\.xlarge | 3 | 20 | 
| r6in\.2xlarge | 3 | 40 | 
| r6in\.4xlarge | 7 | 60 | 
| r6in\.8xlarge | 7 | 90 | 
| r6in\.12xlarge | 7 | 120 | 
| r6in\.16xlarge | 14 | 120 | 
| r6in\.24xlarge | 14 | 120 | 
| r6in\.32xlarge | 13 | 120 | 
| r6id\.large | 2 | 10 | 
| r6id\.xlarge | 3 | 20 | 
| r6id\.2xlarge | 3 | 40 | 
| r6id\.4xlarge | 7 | 60 | 
| r6id\.8xlarge | 7 | 90 | 
| r6id\.12xlarge | 7 | 120 | 
| r6id\.16xlarge | 14 | 120 | 
| r6id\.24xlarge | 14 | 120 | 
| r6id\.32xlarge | 14 | 120 | 
| r6id\.metal | 14 | 120 | 
| u\-3tb1\.56xlarge | 7 | 12 | 
| u\-18tb1\.metal | 14 | 12 | 
| u\-24tb1\.metal | 14 | 12 | 
| x2gd\.medium | 1 | 10 | 
| x2gd\.large | 2 | 10 | 
| x2gd\.xlarge | 3 | 20 | 
| x2gd\.2xlarge | 3 | 40 | 
| x2gd\.4xlarge | 7 | 60 | 
| x2gd\.8xlarge | 7 | 60 | 
| x2gd\.12xlarge | 7 | 60 | 
| x2gd\.16xlarge | 14 | 120 | 
| x2gd\.metal | 14 | 120 | 
| x2idn\.16xlarge | 14 | 120 | 
| x2idn\.24xlarge | 14 | 120 | 
| x2idn\.32xlarge | 14 | 120 | 
| x2idn\.metal | 14 | 120 | 
| x2iedn\.xlarge | 3 | 13 | 
| x2iedn\.2xlarge | 3 | 29 | 
| x2iedn\.4xlarge | 7 | 60 | 
| x2iedn\.8xlarge | 7 | 120 | 
| x2iedn\.16xlarge | 14 | 120 | 
| x2iedn\.24xlarge | 14 | 120 | 
| x2iedn\.32xlarge | 14 | 120 | 
| x2iedn\.metal | 14 | 120 | 
| x2iezn\.2xlarge | 3 | 64 | 
| x2iezn\.4xlarge | 7 | 120 | 
| x2iezn\.6xlarge | 7 | 120 | 
| x2iezn\.8xlarge | 7 | 120 | 
| x2iezn\.12xlarge | 14 | 120 | 
| x2iezn\.metal | 14 | 120 | 

### Storage optimized<a name="eni-branch-so"></a>


| Instance type | Task limit without ENI trunking | Task limit with ENI trunking | 
| --- | --- | --- | 
| i4i\.large | 2 | \-2 | 
| i4i\.xlarge | 3 | 8 | 
| i4i\.2xlarge | 3 | 28 | 
| i4i\.4xlarge | 7 | 58 | 
| i4i\.8xlarge | 7 | 118 | 
| i4i\.16xlarge | 14 | 248 | 
| i4i\.32xlarge | 14 | 498 | 
| i4i\.metal | 14 | 498 | 
| im4gn\.large | 2 | 10 | 
| im4gn\.xlarge | 3 | 20 | 
| im4gn\.2xlarge | 3 | 40 | 
| im4gn\.4xlarge | 7 | 60 | 
| im4gn\.8xlarge | 7 | 60 | 
| im4gn\.16xlarge | 14 | 120 | 
| is4gen\.medium | 1 | 4 | 
| is4gen\.large | 2 | 10 | 
| is4gen\.xlarge | 3 | 20 | 
| is4gen\.2xlarge | 3 | 40 | 
| is4gen\.4xlarge | 7 | 60 | 
| is4gen\.8xlarge | 7 | 60 | 

### Accelerated computing<a name="eni-branch-ac"></a>


| Instance type | Task limit without ENI trunking | Task limit with ENI trunking | 
| --- | --- | --- | 
| dl1\.24xlarge | 59 | 120 | 
| g4ad\.xlarge | 1 | 12 | 
| g4ad\.2xlarge | 1 | 12 | 
| g4ad\.4xlarge | 2 | 12 | 
| g4ad\.8xlarge | 3 | 12 | 
| g4ad\.16xlarge | 7 | 12 | 
| g5\.xlarge | 3 | 6 | 
| g5\.2xlarge | 3 | 19 | 
| g5\.4xlarge | 7 | 40 | 
| g5\.8xlarge | 7 | 90 | 
| g5\.12xlarge | 14 | 120 | 
| g5\.16xlarge | 7 | 120 | 
| g5\.24xlarge | 14 | 120 | 
| g5\.48xlarge | 6 | 120 | 
| g5g\.xlarge | 3 | 20 | 
| g5g\.2xlarge | 3 | 40 | 
| g5g\.4xlarge | 7 | 60 | 
| g5g\.8xlarge | 7 | 60 | 
| g5g\.16xlarge | 14 | 120 | 
| g5g\.metal | 14 | 120 | 
| p3\.2xlarge | 3 | 40 | 
| p3\.8xlarge | 7 | 60 | 
| p3\.16xlarge | 7 | 120 | 
| p4d\.24xlarge | 59 | 120 | 
| p4de\.24xlarge | 59 | 120 | 
| trn1\.2xlarge | 3 | 19 | 
| trn1\.32xlarge | 39 | 120 | 
| vt1\.3xlarge | 3 | 40 | 
| vt1\.6xlarge | 7 | 60 | 
| vt1\.24xlarge | 14 | 120 | 