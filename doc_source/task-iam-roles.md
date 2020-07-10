# IAM Roles for Tasks<a name="task-iam-roles"></a>

With IAM roles for Amazon ECS tasks, you can specify an IAM role that can be used by the containers in a task\. Applications must sign their AWS API requests with AWS credentials, and this feature provides a strategy for managing credentials for your applications to use, similar to the way that Amazon EC2 instance profiles provide credentials to EC2 instances\. Instead of creating and distributing your AWS credentials to the containers or using the EC2 instance’s role, you can associate an IAM role with an ECS task definition or `RunTask` API operation\. The applications in the task’s containers can then use the AWS SDK or CLI to make API requests to authorized AWS services\.

**Important**  
Containers that are running on your container instances are not prevented from accessing the credentials that are supplied to the container instance profile \(through the Amazon EC2 instance metadata server\)\. We recommend that you limit the permissions in your container instance role to the minimal list of permissions shown in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.  
To prevent containers in tasks that use the `awsvpc` network mode from accessing the credential information supplied to the container instance profile \(while still allowing the permissions that are provided by the task role\), set the `ECS_AWSVPC_BLOCK_IMDS` agent configuration variable to `true` in the agent configuration file and restart the agent\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.  
To prevent containers in tasks that use the `bridge` network mode from accessing the credential information supplied to the container instance profile \(while still allowing the permissions that are provided by the task role\) by running the following iptables command on your container instances\. Note that this command does not affect containers in tasks that use the `host` or `awsvpc` network modes\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.  

```
sudo yum install -y iptables-services; sudo iptables --insert FORWARD 1 --in-interface docker+ --destination 169.254.169.254/32 --jump DROP
```
You must save this iptables rule on your container instance for it to survive a reboot\. For the Amazon ECS\-optimized AMI, use the following command\. For other operating systems, consult the documentation for that OS\.  
For the Amazon ECS\-optimized Amazon Linux 2 AMI:  

  ```
  sudo iptables-save | sudo tee /etc/sysconfig/iptables && sudo systemctl enable --now iptables
  ```
For the Amazon ECS\-optimized Amazon Linux AMI:  

  ```
  sudo service iptables save
  ```

You define the IAM role to use in your task definitions, or you can use a `taskRoleArn` override when running a task manually with the `RunTask` API operation\. The Amazon ECS agent receives a payload message for starting the task with additional fields that contain the role credentials\. The Amazon ECS agent sets a unique task credential ID as an identification token and updates its internal credential cache so that the identification token for the task points to the role credentials that are received in the payload\. The Amazon ECS agent populates the `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` environment variable in the `Env` object \(available with the docker inspect *container\_id* command\) for all containers that belong to this task with the following relative URI: `/credential_provider_version/credentials?id=task_credential_id`\. 

**Note**  
When you specify an IAM role for a task, the AWS CLI or other SDKs in the containers for that task use the AWS credentials provided by the task role exclusively and they no longer inherit any IAM permissions from the container instance\.

From inside the container, you can query the credentials with the following command:

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
The default expiration time for the generated IAM role credentials is 6 hours\.

If your container instance is using at least version 1\.11\.0 of the container agent and a supported version of the AWS CLI or SDKs, then the SDK client will see that the `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` variable is available, and it will use the provided credentials to make calls to the AWS APIs\. For more information, see [Enabling Task IAM Roles on your Container Instances](#enable_task_iam_roles) and [Using a Supported AWS SDK](#task-iam-roles-minimum-sdk)\.

Each time the credential provider is used, the request is logged locally on the host container instance at `/var/log/ecs/audit.log.YYYY-MM-DD-HH`\. For more information, see [IAM Roles for Tasks Credential Audit Log](logs.md#task_iam_roles-logs)\.

**Topics**
+ [Benefits of Using IAM Roles for Tasks](#task_im_roles_benefits)
+ [Enabling Task IAM Roles on your Container Instances](#enable_task_iam_roles)
+ [Creating an IAM Role and Policy for your Tasks](#create_task_iam_policy_and_role)
+ [Using a Supported AWS SDK](#task-iam-roles-minimum-sdk)
+ [Specifying an IAM Role for your Tasks](#specify-task-iam-roles)

## Benefits of Using IAM Roles for Tasks<a name="task_im_roles_benefits"></a>
+ **Credential Isolation:** A container can only retrieve credentials for the IAM role that is defined in the task definition to which it belongs; a container never has access to credentials that are intended for another container that belongs to another task\.
+ **Authorization:** Unauthorized containers cannot access IAM role credentials defined for other tasks\.
+ **Auditability:** Access and event logging is available through CloudTrail to ensure retrospective auditing\. Task credentials have a context of `taskArn` that is attached to the session, so CloudTrail logs show which task is using which role\.

## Enabling Task IAM Roles on your Container Instances<a name="enable_task_iam_roles"></a>

Your Amazon ECS container instances require at least version 1\.11\.0 of the container agent to enable task IAM roles; however, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\. If you are using the Amazon ECS\-optimized AMI, your instance needs at least 1\.11\.0\-1 of the `ecs-init` package\. If your container instances are launched from version 2016\.03\.e or later, then they contain the required versions of the container agent and `ecs-init`\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

If you are not using the Amazon ECS\-optimized AMI for your container instances, be sure to add the `--net=host` option to your docker run command that starts the agent and the appropriate agent configuration variables for your desired configuration \(for more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\):

`ECS_ENABLE_TASK_IAM_ROLE=true`  
Enables IAM roles for tasks for containers with the `bridge` and `default` network modes\.

`ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true`  
Enables IAM roles for tasks for containers with the `host` network mode\. This variable is only supported on agent versions 1\.12\.0 and later\.

For an example run command, see [Manually Updating the Amazon ECS Container Agent \(for Non\-Amazon ECS\-Optimized AMIs\)](manually_update_agent.md)\. You will also need to set the following networking commands on your container instance so that the containers in your tasks can retrieve their AWS credentials:

```
sudo sysctl -w net.ipv4.conf.all.route_localnet=1
sudo iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
sudo iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679
```

You must save these iptables rules on your container instance for them to survive a reboot\. You can use the iptables\-save and iptables\-restore commands to save your iptables rules and restore them at boot\. For more information, consult your specific operating system documentation\.

## Creating an IAM Role and Policy for your Tasks<a name="create_task_iam_policy_and_role"></a>

You must create an IAM policy for your tasks to use that specifies the permissions that you would like the containers in your tasks to have\. You have several ways to create a new IAM permission policy\. You can copy a complete AWS managed policy that already does some of what you're looking for and then customize it to your specific requirements\. For more information, see [Creating a New Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

You must also create a role for your tasks to use before you can specify it in your task definitions\. You can create the role using the **Amazon Elastic Container Service Task Role** service role in the IAM console\. Then you can attach your specific IAM policy to the role that gives the containers in your task the permissions you desire\. The procedures below describe how to do this\.

If you have multiple task definitions or services that require IAM permissions, you should consider creating a role for each specific task definition or service with the minimum required permissions for the tasks to operate so that you can minimize the access that you provide for each task\. 

The Amazon ECS Task Role trust relationship is shown below\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

**To create an IAM policy for your tasks**

In this example, we create a policy to allow read\-only access to an Amazon S3 bucket\. You could store database credentials or other secrets in this bucket, and the containers in your task can read the credentials from the bucket and load them into your application\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and then choose **Create policy**\. 

1. Follow the steps under one of the following tabs, which shows you how to use the visual or JSON editors\.

------
#### [ Using the visual editor ]

1. For **Service**, choose **S3**\.

1. For **Actions**, expand the **Read** option and select **GetObject**\.

1. For **Resources**, select **Add ARN** and enter the full Amazon Resource Name \(ARN\) of your Amazon S3 bucket, and then choose **Review policy**\.

1. On the **Review policy** page, for **Name** type your own unique name, such as `AmazonECSTaskS3BucketPolicy`\.

1. Choose **Create policy** to finish\.

------
#### [ Using the JSON editor ]

1. In the **Policy Document** field, paste the policy to apply to your tasks\. The example below allows permission to the *my\-task\-secrets\-bucket* Amazon S3 bucket\. You can modify the policy document to suit your specific needs\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetObject"
         ],
         "Resource": [
           "arn:aws:s3:::my-task-secrets-bucket/*"
         ]
       }
     ]
   }
   ```

1. Choose **Create policy**\. 

------

**To create an IAM role for your tasks**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. For **Select type of trusted entity** section, choose **AWS service**\.

1. For **Choose the service that will use this role**, choose **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task** and choose **Next: Permissions**\.

1. For **Attach permissions policy**, select the policy to use for your tasks \(in this example `AmazonECSTaskS3BucketPolicy`, and then choose **Next: Tags**\.

1. For **Add tags \(optional\)**, enter any metadata tags you want to associate with the IAM role, and then choose **Next: Review**\.

1. For **Role name**, enter a name for your role\. For this example, type `AmazonECSTaskS3BucketRole` to name the role, and then choose **Create role** to finish\.

## Using a Supported AWS SDK<a name="task-iam-roles-minimum-sdk"></a>

Support for IAM roles for tasks was added to the AWS SDKs on July 13th, 2016\. The containers in your tasks must use an AWS SDK version that was created on or after that date\. AWS SDKs that are included in Linux distribution package managers may not be new enough to support this feature\.

To ensure that you are using a supported SDK, follow the installation instructions for your preferred SDK at [Tools for Amazon Web Services](https://aws.amazon.com/tools/) when you are building your containers to get the latest version\.

## Specifying an IAM Role for your Tasks<a name="specify-task-iam-roles"></a>

After you have created a role and attached a policy to that role, you can run tasks that assume the role\. You have several options to do this:
+ Specify an IAM role for your tasks in the task definition\. You can create a new task definition or a new revision of an existing task definition and specify the role you created previously\. If you use the console to create your task definition, choose your IAM role in the **Task Role** field\. If you use the AWS CLI or SDKs, specify your task role ARN using the `taskRoleArn` parameter\. For more information, see [Creating a task definition](create-task-definition.md)\.
**Note**  
This option is required if you want to use IAM task roles in an Amazon ECS service\.
+ Specify an IAM task role override when running a task\. You can specify an IAM task role override when running a task\. If you use the console to run your task, choose **Advanced Options** and then choose your IAM role in the **Task Role** field\. If you use the AWS CLI or SDKs, specify your task role ARN using the `taskRoleArn` parameter in the `overrides` JSON object\. For more information, see [Running tasks](ecs_run_task.md)\. 

**Note**  
In addition to the standard Amazon ECS permissions required to run tasks and services, IAM users also require `iam:PassRole` permissions to use IAM roles for tasks\.