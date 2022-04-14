# IAM roles for tasks<a name="task-iam-roles"></a>

Your Amazon ECS tasks can have an IAM role associated with them\. The permissions granted in the IAM role are assumed by the containers running in the task\.

If your containerized applications need to call AWS APIs, they must sign their AWS API requests with AWS credentials, and a task IAM role provides a strategy for managing credentials for your applications to use, similar to the way that an Amazon EC2 instance profile provides credentials to Amazon EC2 instances\. Instead of creating and distributing your AWS credentials to the containers or using the Amazon EC2 instance’s role, you can associate an IAM role with an Amazon ECS task definition or `RunTask` API operation\. Your containers can then use the AWS SDK or AWS CLI to make API requests to authorized AWS services\.

The following explain the benefits of using IAM roles with your tasks\.
+ **Credential Isolation:** A container can only retrieve credentials for the IAM role that is defined in the task definition to which it belongs; a container never has access to credentials that are intended for another container that belongs to another task\.
+ **Authorization:** Unauthorized containers cannot access IAM role credentials defined for other tasks\.
+ **Auditability:** Access and event logging is available through CloudTrail to ensure retrospective auditing\. Task credentials have a context of `taskArn` that is attached to the session, so CloudTrail logs show which task is using which role\.

**Note**  
When you specify an IAM role for a task, the AWS CLI or other SDKs in the containers for that task use the AWS credentials provided by the task role exclusively and they no longer inherit any IAM permissions from the Amazon EC2 or external instance they are running on\.

## Considerations<a name="task-iam-role-considerations"></a>

The following notes should be considered when using an IAM role with your tasks\.
+ Containers that are running as part of a task on an Amazon EC2 instance are not prevented from accessing the credentials that are supplied to the Amazon EC2 instance profile \(through the Amazon EC2 instance metadata server\)\. We recommend that you limit the permissions in your container instance role to the minimal list of permissions shown in [Amazon ECS container instance IAM role](instance_IAM_role.md)\.
+ To prevent containers run by tasks that use the `awsvpc` network mode from accessing the credential information supplied to the Amazon EC2 instance profile, while still allowing the permissions that are provided by the task role, set the `ECS_AWSVPC_BLOCK_IMDS` agent configuration variable to `true` in the agent configuration file and restart the agent\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.
+ To prevent containers run by tasks that use the `bridge` network mode from accessing the credential information supplied to the Amazon EC2 instance profile, while still allowing the permissions that are provided by the task role, by running the following iptables command on your Amazon EC2 instances\. This command doesn't affect containers in tasks that use the `host` or `awsvpc` network modes\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.

  ```
  sudo yum install -y iptables-services; sudo iptables --insert DOCKER-USER 1 --in-interface docker+ --destination 169.254.169.254/32 --jump DROP
  ```

  You must save this iptables rule on your Amazon EC2 instance for it to survive a reboot\. When using the Amazon ECS\-optimized AMI, you can use the following command\. For other operating systems, consult the documentation for that operating system\.

  ```
  sudo iptables-save | sudo tee /etc/sysconfig/iptables && sudo systemctl enable --now iptables
  ```
+ You can specify a task IAM role in your task definitions, or you can use a `taskRoleArn` override when running a task manually with the `RunTask` API operation\. The Amazon ECS agent receives a payload message for starting the task with additional fields that contain the role credentials\. The Amazon ECS agent sets a unique task credential ID as an identification token and updates its internal credential cache so that the identification token for the task points to the role credentials that are received in the payload\. The Amazon ECS agent populates the `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` environment variable in the `Env` object \(available with the docker inspect *container\_id* command\) for all containers that belong to this task with the following relative URI: `/credential_provider_version/credentials?id=task_credential_id`\.

  From inside the container, you can query the credential endpoint with the following command:

  ```
  curl 169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI
  ```

  Output:

  ```
  {
      "AccessKeyId": "ACCESS_KEY_ID",
      "Expiration": "EXPIRATION_DATE",
      "RoleArn": "TASK_ROLE_ARN",
      "SecretAccessKey": "SECRET_ACCESS_KEY",
      "Token": "SECURITY_TOKEN_STRING"
  }
  ```
**Note**  
The default expiration time for the generated IAM role credentials is 6 hours\. The expiration time format is the simple date format\. For information about how to modify the expiration time, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\.
+ If your Amazon EC2 instance is using at least version `1.11.0` of the container agent and a supported version of the AWS CLI or SDKs, then the SDK client will see that the `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` variable is available, and it will use the provided credentials to make calls to the AWS APIs\. For more information, see [Enabling task IAM roles on your Amazon EC2 or external instances](#enable_task_iam_roles) and [Using a supported AWS SDK](#task-iam-roles-minimum-sdk)\.

  Each time the credential provider is used, the request is logged locally on the host container instance at `/var/log/ecs/audit.log.YYYY-MM-DD-HH`\. For more information, see [IAM Roles for Tasks Credential Audit Log](logs.md#task_iam_roles-logs)\.
+ When creating your task IAM role, it is recommended that you use the `aws:SourceAccount` or `aws:SourceArn` condition keys in either the trust relationship or the IAM policy associated with the role to prevent the confused deputy security issue\. These condition keys can be specified in the trust relationship or in the IAM policy associated with the role\. To learn more about the confused deputy problem and how to protect your AWS account, see [The confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) in the *IAM User Guide*\.

## Enabling task IAM roles on your Amazon EC2 or external instances<a name="enable_task_iam_roles"></a>

Your Amazon EC2 or external instances require at least version `1.11.0` of the container agent to enable task IAM roles; however, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\. If you are using an Amazon ECS\-optimized AMI, your instance needs at least `1.11.0-1` of the `ecs-init` package\. If your instances are using the latest Amazon ECS\-optimized AMI, then they contain the required versions of the container agent and `ecs-init`\. For more information, see [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)\.

If you are not using the Amazon ECS\-optimized AMI for your container instances, be sure to add the `--net=host` option to your docker run command that starts the agent and the following agent configuration variables for your desired configuration \(for more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\):

`ECS_ENABLE_TASK_IAM_ROLE=true`  
Enables IAM roles for tasks for containers with the `bridge` and `default` network modes\.

`ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true`  
Enables IAM roles for tasks for containers with the `host` network mode\. This variable is only supported on agent versions 1\.12\.0 and later\.

For an example run command, see [Manually updating the Amazon ECS container agent \(for non\-Amazon ECS\-Optimized AMIs\)](manually_update_agent.md)\. You will also need to set the following networking commands on your container instance so that the containers in your tasks can retrieve their AWS credentials:

```
sudo sysctl -w net.ipv4.conf.all.route_localnet=1
sudo iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
sudo iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679
```

You must save these iptables rules on your container instance for them to survive a reboot\. You can use the iptables\-save and iptables\-restore commands to save your iptables rules and restore them at boot\. For more information, consult your specific operating system documentation\.

## Creating an IAM role and policy for your tasks<a name="create_task_iam_policy_and_role"></a>

When creating an IAM policy for your tasks to use, the policy should include the permissions that you would like the containers in your tasks to assume\. You can use an existing AWS managed policy that as an example or you can create a custom policy from scratch that meets your specific needs\. For more information, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

Once the IAM policy is created, you can create an IAM role which includes that policy which you reference in your Amazon ECS task definition\. You can create the role using the **Elastic Container Service Task** use case in the IAM console\. Then you can attach your specific IAM policy to the role that gives the containers in your task the permissions you desire\. The procedures below describe how to do this\.

If you have multiple task definitions or services that require IAM permissions, you should consider creating a role for each specific task definition or service with the minimum required permissions for the tasks to operate so that you can minimize the access that you provide for each task\. 

For information about the service endpoint for your Region, see [Service endpoints](https://docs.aws.amazon.com/general/latest/gr/ecs-service.html#ecs_region) in the *Amazon Web Services General Reference Reference Guide*\.

The IAM task role must have a trust policy that specifies the `ecs-task.amazonaws.com` service\. The `sts:AssumeRole` permission allows your tasks to assume an IAM role that's different from the one that the Amazon EC2 instance uses\. This way, your task doesn't inherit the role associated with the Amazon EC2 instance\. It is recommended that you use the `aws:SourceAccount` or `aws:SourceArn` condition keys to scope the permissions further to prevent the confused deputy security issue\. These condition keys can be specified in the trust relationship or in the IAM policy associated with the role\. To learn more about the confused deputy problem and how to protect your AWS account, see [The confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) in the *IAM User Guide*\.

The following is an example trust policy\. You should replace the Region identifier and specify the AWS account number that you use when launching tasks\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Principal":{
            "Service":[
               "ecs-tasks.amazonaws.com"
            ]
         },
         "Action":"sts:AssumeRole",
         "Condition":{
            "ArnLike":{
               "aws:SourceArn":"aws:ecs:us-west-2:111122223333:*"
            },
            "StringEquals":{
               "aws:SourceAccount":"111122223333"
            }
         }
      }
   ]
}
```

**To create an IAM policy for your tasks \(AWS Management Console\)**

In this example, we create a policy to allow read\-only access to an Amazon S3 bucket\. You could store database credentials or other secrets in this bucket, and the containers in your task can read the credentials from the bucket and load them into your application\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and then choose **Create Policy**\. 

1. Follow the steps under one of the following tabs, which shows you how to use the visual or JSON editors\.

------
#### [ Using the visual editor ]

1. For **Service**, choose **S3**\.

1. For **Actions**, expand the **Read** option and select **GetObject**\.

1. For **Resources**, select **Add ARN** and enter the full Amazon Resource Name \(ARN\) of your Amazon S3 bucket\.

1. \(Optional\) For **Request conditions**, select **Add condition**\. This is recommended to prevent the confused deputy security issue\. To learn more about the confused deputy problem and how to protect your AWS account, see [The confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) in the *IAM User Guide*\.

   1. For **Condition key**, select either **aws:SourceAccount** or **aws:SourceArn**\. For more information about these global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

   1. For **Operator**, select **StringEquals** if you specified the **aws:SourceAccount** condition key or **ArnLike** if you specified the **aws:SourceArn** condition key\.

   1. For **Value**, specify your AWS account ID if you specified the **aws:SourceAccount** condition key or the Amazon Resource Name \(ARN\) of your Amazon ECS task if you specified the **aws:SourceArn** condition key\. You may use wildcards, for example `aws:ecs:*:accountId:*` which will work for all tasks in your account\.

   1. Choose **Add** to save the condition key\. Repeat these steps for each condition key you want to add to the policy\.

1. Choose **Next: Tags** and add any resource tags to the policy to help you organize them and then choose **Next: Review**\.

1. On the **Review policy** page, for **Name** type your own unique name, such as `AmazonECSTaskS3BucketPolicy`\. You may specify an optional description for the policy as well\.

1. When the policy is complete, choose **Create policy** to finish\.

------
#### [ Using the JSON editor ]

1. In the policy document field, paste the policy to apply to your tasks\. The example below allows permission to the *my\-task\-secrets\-bucket* Amazon S3 bucket\. It includes a condition statement, which you can use to specify either a specific task using its Amazon Resource Name \(ARN\) or a specific account ID\. This provides a way to further scope the permission for additional security\. This is recommended to prevent the confused deputy security issue\. To learn more about the confused deputy problem and how to protect your AWS account, see [The confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) in the *IAM User Guide*\.

   The following is an example permissions policy\. You can modify the policy to suit your specific needs\. You should replace the Region identifier and specify the AWS account number that you use when launching tasks\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":[
               "s3:GetObject"
            ],
            "Resource":[
               "arn:aws:s3:::my-task-secrets-bucket/*"
            ],
            "Condition":{
               "ArnLike":{
                  "aws:SourceArn":"aws:ecs:us-west-2:111122223333:*"
               },
               "StringEquals":{
                  "aws:SourceAccount":"111122223333"
               }
            }
         }
      ]
   }
   ```

1. Choose **Next: Tags** and add any resource tags to the policy to help you organize them and then choose **Next: Review**\.

1. On the **Review policy** page, for **Name** type your own unique name, such as `AmazonECSTaskS3BucketPolicy`\. You may specify an optional description for the policy as well\.

1. When the policy is complete, choose **Create policy** to finish\.

------

**To create an IAM role for your tasks \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. For **Select trusted entity** section, choose **AWS service**\.

1. For **Use case**, using the drop down menu, select **Elastic Container Service** and then the **Elastic Container Service Task** use case and then choose **Next**\.

1. For **Add permissions**, search for and select the policy to use for your tasks \(in this example `AmazonECSTaskS3BucketPolicy`, and then choose **Next**\.

1. On **Step 3: Name, review, and create**, do the following:

   1. For **Role name**, enter a name for your role\. For this example, type `AmazonECSTaskS3BucketRole` to name the role\.

   1. \(Optional\) For **Description**\. specify a description for this IAM role\.

   1. Review the trusted entity and permissions policy for the role\.

   1. For **Add tags \(Optional\)**, enter any metadata tags you want to associate with the IAM role, and then choose **Create role**\.

## Using a supported AWS SDK<a name="task-iam-roles-minimum-sdk"></a>

Support for IAM roles for tasks was added to the AWS SDKs on July 13th, 2016\. The containers in your tasks must use an AWS SDK version that was created on or after that date\. AWS SDKs that are included in Linux distribution package managers may not be new enough to support this feature\.

To ensure that you are using a supported SDK, follow the installation instructions for your preferred SDK at [Tools for Amazon Web Services](https://aws.amazon.com/tools/) when you are building your containers to get the latest version\.

## Specifying an IAM role for your tasks<a name="specify-task-iam-roles"></a>

After you have created a role and attached a policy to that role, you can run tasks that assume the role\. You have several options to do this:
+ Specify an IAM role for your tasks in the task definition\. You can create a new task definition or a new revision of an existing task definition and specify the role you created previously\. If you use the classic console to create your task definition, choose your IAM role in the **Task Role** field\. If you use the AWS CLI or SDKs, specify the Amazon Resource Name \(ARN\) of your task role using the `taskRoleArn` parameter\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.
**Note**  
This option is required if you want to use IAM task roles in an Amazon ECS service\.
+ Specify an IAM task role override when running a task\. You can specify an IAM task role override when running a task\. If you use the classic console to run your task, choose **Advanced Options** and then choose your IAM role in the **Task Role** field\. If you use the AWS CLI or SDKs, specify your task role ARN using the `taskRoleArn` parameter in the `overrides` JSON object\. For more information, see [Run a standalone taskRun a standalone task using the new console](ecs_run_task.md)\. 

**Note**  
In addition to the standard Amazon ECS permissions required to run tasks and services, IAM users also require `iam:PassRole` permissions to use IAM roles for tasks\.