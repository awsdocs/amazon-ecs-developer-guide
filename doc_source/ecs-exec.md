# Using Amazon ECS Exec for debugging<a name="ecs-exec"></a>

With Amazon ECS Exec, you can directly interact with containers without needing to first interact with the host container operating system, open inbound ports, or manage SSH keys\. You can use ECS Exec to run commands in or get a shell to a container running on an Amazon EC2 instance or on AWS Fargate\. This makes it easier to collect diagnostic information and quickly troubleshoot errors\. For example, in a development context, you can use ECS Exec to easily interact with various process in your containers and troubleshoot your applications\. And, in production scenarios, you can use it to gain break\-glass access to your containers to debug issues\. 

You can run commands in a running Linux or Windows container using ECS Exec from the Amazon ECS API, AWS Command Line Interface \(AWS CLI\), AWS SDKs, or the AWS Copilot CLI\. For details on using ECS Exec, as well as a video walkthrough, using the AWS Copilot CLI, see the [Copilot Github documentation](https://aws.github.io/copilot-cli/docs/commands/svc-exec/)\.

You can also use ECS Exec to maintain stricter access control policies and audit container access\. By selectively turning on this feature, you can control who can run commands and on which tasks they can run those commands\. With a log of each command and their output, you can use ECS Exec to audit which tasks were run and you can use CloudTrail to audit who accessed a container\.

## Architecture<a name="ecs-exec-architecture"></a>

ECS Exec makes use of AWS Systems Manager \(SSM\) Session Manager to establish a connection with the running container and uses AWS Identity and Access Management \(IAM\) policies to control access to running commands in a running container\. This is made possible by bind\-mounting the necessary SSM agent binaries into the container\. The Amazon ECS or AWS Fargate agent is responsible for starting the SSM core agent inside the container alongside your application code\. For more information, see [Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)\.

You can audit which user accessed the container using AWS CloudTrail and log each command \(and their output\) to Amazon S3 or Amazon CloudWatch Logs\. To encrypt data between the local client and container with your own encryption key, you must provide the AWS Key Management Service \(AWS KMS\) key\.



## Considerations for using ECS Exec<a name="ecs-exec-considerations"></a>

For this topic, you should be familiar with the following aspects involved with using ECS Exec:
+ ECS Exec is supported for AWS Fargate, external instances \(ECS Anywhere\), Linux containers hosted on Amazon EC2 and the following Windows Amazon ECS\-optimized AMIs \(with the container agent version `1.56` or later\):
  + Amazon ECS\-optimized Windows Server 2022 Full AMI
  + Amazon ECS\-optimized Windows Server 2022 Core AMI
  + Amazon ECS\-optimized Windows Server 2019 Full AMI
  + Amazon ECS\-optimized Windows Server 2019 Core AMI
  + Amazon ECS\-optimized Windows Server 20H2 Core AMI
+ ECS Exec is not currently supported using the AWS Management Console\.
+ ECS Exec is not currently supported for tasks launched using an Auto Scaling group capacity provider\.
+ If you are using interface Amazon VPC endpoints with Amazon ECS, you must create the interface Amazon VPC endpoints for Systems Manager Session Manager\. For more information, see [Create the Systems Manager Session Manager VPC endpoints when using the ECS Exec feature](vpc-endpoints.md#ecs-vpc-endpoint-ecsexec)\.
+ You can't enable ECS Exec for existing tasks\. It can only be enabled for new tasks\.
+ When a user runs commands on a container using ECS Exec, these commands are run as the `root` user\. The SSM agent and its child processes run as root even when you specify a user ID for the container\.
+ The ECS Exec session has an idle timeout time of 20 minutes\. This value cannot be changed\.
+ The SSM agent requires that the container file system is able to be written to in order to create the required directories and files\. Therefore, making the root file system read\-only using the `readonlyRootFilesystem` task definition parameter, or any other method, isn't supported\.
+ Users can run all of the commands that are available within the container context\. The following actions might result in orphaned and zombie processes: terminating the main process of the container, terminating the command agent, and deleting dependencies\. To cleanup zombie processes, we recommend adding the `initProcessEnabled` flag to your task definition\.
+ While starting SSM sessions outside of the `execute-command` action is possible, this results in the sessions not being logged and being counted against the session limit\. We recommend limiting this access by denying the `ssm:start-session` action using an IAM policy\. For more information, see [Limiting access to the Start Session action](#ecs-exec-limit-access-start-session)\.
+ ECS Exec will use some CPU and memory\. You'll want to accommodate for that when specifying the CPU and memory resource allocations in your task definition\.
+ You must be using AWS CLI version `1.22.3` or later or AWS CLI version `2.3.6` or later\. For information about how to update the AWS CLI, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) in the *AWS Command Line Interface User Guide Version 2*\.
+ You cannot use ECS Exec when you use `run-task` to launch a task on a cluster that uses managed scaling with asynchronous placement \(launch a task with no instance\)\.
+ You cannot run ECS Exec against Microsoft Nano Server containers\. For more information about Nano Server containers, see [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver) on the Docker web site\.

## Prerequisites for using ECS Exec<a name="ecs-exec-prerequisites"></a>

Before you start using ECS Exec, make sure you that you have completed these actions:
+ Install and configure the AWS CLI\. For more information, see [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)\.
+ Install Session Manager plugin for the AWS CLI\. For more information, see [Install the Session Manager plugin for the AWS CLI](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)\.
+ ECS Exec has version requirements depending on whether your tasks are hosted on Amazon EC2 or AWS Fargate:
  + If you're using Amazon EC2, you must use an Amazon ECS optimized AMI that was released after January 20th, 2021, with an agent version of 1\.50\.2 or greater\. For more information, see [Amazon ECS optimized AMIs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html)\.
  + If you're using AWS Fargate, you must use platform version `1.4.0` or higher \(Linux\) or `1.0.0` \(Windows\)\. For more information, see [AWS Fargate platform versions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/platform_versions.html)\.

## Enabling and using ECS Exec<a name="ecs-exec-enabling-and-using"></a>

### IAM permissions required for ECS Exec<a name="ecs-exec-required-iam-permissions"></a>

The ECS Exec feature requires a task IAM role to grant containers the permissions needed for communication between the managed SSM agent \(`execute-command` agent\) and the SSM service\. For more information, see [Amazon ECS task IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)\. You should add the following permissions to a task IAM role and include the task IAM role in your task definition\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.

Use the following policy for your task IAM role to add the required SSM permissions\.

```
{
   "Version": "2012-10-17",
   "Statement": [
       {
       "Effect": "Allow",
       "Action": [
            "ssmmessages:CreateControlChannel",
            "ssmmessages:CreateDataChannel",
            "ssmmessages:OpenControlChannel",
            "ssmmessages:OpenDataChannel"
       ],
      "Resource": "*"
      }
   ]
}
```

### Optional task definition changes<a name="ecs-exec-task-definition"></a>

If you set the task definition parameter `initProcessEnabled` to `true`, this starts the init process inside the container, which removes any zombie SSM agent child processes found\. The following provides an example\.

```
{
    "taskRoleArn": "ecsTaskRole",
    "networkMode": "awsvpc",
    "requiresCompatibilities": [
        "EC2",
        "FARGATE"
    ],
    "executionRoleArn": "ecsTaskExecutionRole",
    "memory": ".5 gb",
    "cpu": ".25 vcpu",
    "containerDefinitions": [
        {
            "name": "amazon-linux",
            "image": "amazonlinux:latest",
            "essential": true,
            "command": ["sleep","3600"],
            "linuxParameters": {
                "initProcessEnabled": true
            }
        }
    ],
    "family": "ecs-exec-task"
}
```

### Enabling ECS Exec for your tasks and services<a name="ecs-exec-enabling"></a>

You can enable the ECS Exec feature for your services and standalone tasks by specifying the `--enable-execute-command` flag when using one of the following AWS CLI commands: [https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html), [https://docs.aws.amazon.com/cli/latest/reference/ecs/update-service-console-v2.html](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-service-console-v2.html), [https://docs.aws.amazon.com/cli/latest/reference/ecs/start-task.html](https://docs.aws.amazon.com/cli/latest/reference/ecs/start-task.html), or [https://docs.aws.amazon.com/cli/latest/reference/ecs/run-task.html](https://docs.aws.amazon.com/cli/latest/reference/ecs/run-task.html)\.

For example, if you run the following command, the ECS Exec feature is enabled for a newly created service\. For more information about creating services, see [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html)\.

```
aws ecs create-service \
    --cluster cluster-name \
    --task-definition task-definition-name \
    --enable-execute-command \
    --service-name service-name \
    --desired-count 1
```

After you have enabled ECS Exec for a task, you can run the following command to confirm the task is ready to be used\. If the `lastStatus` property of the `ExecuteCommandAgent` is listed as `RUNNING` and the `enableExecuteCommand` property is set to `true`, then your task is ready\.

```
aws ecs describe-tasks \
    --cluster cluster-name \
    --tasks task-id
```

The following output snippet is an example of what you might see\.

```
{
    "tasks": [
        {
            ...
            "containers": [
                {
                    ...
                    "managedAgents": [
                        {
                            "lastStartedAt": "2021-03-01T14:49:44.574000-06:00",
                            "name": "ExecuteCommandAgent",
                            "lastStatus": "RUNNING"
                        }
                    ]
                }
            ],
            ...
            "enableExecuteCommand": true,
            ...
        }
    ]
}
```

### Running commands using ECS Exec<a name="ecs-exec-running-commands"></a>

After you have confirmed the `ExecuteCommandAgent` is running, you can open an interactive shell on your container using the following command\. If your task contains multiple containers, you must specify the container name using the `--container` flag\. Amazon ECS only supports initiating interactive sessions, so you must use the `--interactive` flag\.

The following command will run an interactive `/bin/sh` command against a container named **container\-name** for a task with an id of *task\-id*\.

```
aws ecs execute-command --cluster cluster-name \
    --task task-id \
    --container container-name \
    --interactive \
    --command "/bin/sh"
```

## Logging and Auditing using ECS Exec<a name="ecs-exec-logging"></a>

### Enabling logging and auditing in your tasks and services<a name="ecs-exec-enabling-logging"></a>

Amazon ECS provides a default configuration for logging commands run using ECS Exec by sending logs to CloudWatch Logs using the `awslogs` log driver that's configured in your task definition\. If you want to provide a custom configuration, the AWS CLI supports a `--configuration` flag for both the `create-cluster` and `update-cluster` commands\. It’s also important to know that the container image requires `script` and `cat` to be installed in order to have command logs uploaded correctly to Amazon S3 or CloudWatch Logs\. For more information about creating clusters, see [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html)\.

**Note**  
This configuration only handles the logging of the `execute-command` session\. It doesn't affect logging of your application\.

The following example creates a service and then logs the output to your CloudWatch Logs LogGroup named `cloudwatch-log-group-name` and your Amazon S3 bucket named `s3-bucket-name`\.

You must use an AWS KMS customer managed key to encrypt the log group when you set the `CloudWatchEncryptionEnabled` option to `true`\. For information about how to encrypt the log group, see [Encrypt log data in CloudWatch Logs using AWS Key Management Service](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html#encrypt-log-data-kms-policy), in the *Amazon CloudWatch Logs User Guide*\.

```
aws ecs create-cluster \
    --cluster-name cluster-name \
    --configuration executeCommandConfiguration="{ \
        kmsKeyId=string, \
        logging=OVERRIDE, \
        logConfiguration={ \
            cloudWatchLogGroupName=cloudwatch-log-group-name, \
            cloudWatchEncryptionEnabled=true, \
            s3BucketName=s3-bucket-name, \
            s3EncryptionEnabled=true, \
            s3KeyPrefix=demo \
        } \
    }"
```

The `logging` property determines the behavior of the logging capability of ECS Exec:
+ `NONE`: logging is disabled
+ `DEFAULT`: logs are sent to the configured `awslogs` driver \(If the driver isn't configured, then no log is saved\.\)
+ `OVERRIDE`: logs are sent to the provided Amazon CloudWatch Logs LogGroup, Amazon S3 bucket, or both

### IAM permissions required for Amazon CloudWatch Logs or Amazon S3 Logging<a name="ecs-exec-required-logging-permissions"></a>

To enable logging, the Amazon ECS task role that's referenced in your task definition needs to have additional permissions\. These additional permissions can be added as an inline policy to the task role\. They are different depending on if you direct your logs to Amazon CloudWatch Logs or Amazon S3\.

------
#### [ Amazon CloudWatch Logs ]

The following example inline policy adds the required Amazon CloudWatch Logs permissions\.

```
{
   "Version": "2012-10-17",
   "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogGroups"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
               "logs:CreateLogStream",
               "logs:DescribeLogStreams",
               "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:region:account-id:log-group:/aws/ecs/cloudwatch-log-group-name:*"
        }
   ]
}
```

------
#### [ Amazon S3 ]

The following example inline policy adds the required Amazon S3 permissions\.

```
{
   "Version": "2012-10-17",
   "Statement": [
        {
            "Effect": "Allow",
            "Action": [
               "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
           "Effect": "Allow",
           "Action": [
               "s3:GetEncryptionConfiguration"
           ],
           "Resource": "arn:aws:s3:::s3-bucket-name"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::s3-bucket-name/*"
        }
    ]
   }
```

------

### IAM permissions required for encryption using your own AWS KMS key \(KMS key\)<a name="ecs-exec-required-kms-permissions"></a>

By default, the data transferred between your local client and the container uses TLS 1\.2 encryption that AWS provides\. To further encrypt data using your own KMS key, you must create a KMS key and add the `kms:Decrypt` permission to your task IAM role\. This permission is used by your container to decrypt the data\. For more information about creating a KMS key, see [Creating keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html)\.

You would add the following inling policy to your task IAM role which requires the AWS KMS permissions\. For more information, see [IAM permissions required for ECS Exec](#ecs-exec-required-iam-permissions)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": "kms-key-arn"
        }
    ]
}
```

For the data to be encrypted using your own KMS key, the user or group using the `execute-command` action must be granted the `kms:GenerateDataKey` permission\.

The following example policy for your user or group contains the required permission to use your own KMS key\. You must specify the Amazon Resource Name \(ARN\) of your KMS key\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Resource": "kms-key-arn"
        }
    ]
}
```

## Using IAM policies to limit access to ECS Exec<a name="ecs-exec-best-practices-limit-access-execute-command"></a>

You can limit user access to the execute\-command API action by using one or more of the following IAM policy condition keys:
+ `aws:ResourceTag/clusterTagKey`
+ `ecs:ResourceTag/clusterTagKey`
+ `aws:ResourceTag/taskTagKey`
+ `ecs:ResourceTag/taskTagKey`
+ `ecs:container-name`
+ `ecs:cluster`
+ `ecs:task`
+ `ecs:enable-execute-command`

With the following example IAM policy, users can run commands in containers that are running within tasks with a tag that has an `environment` key and `development` value and in a cluster that's named `cluster-name`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:ExecuteCommand",
                "ecs:DescribeTasks"
            ],
            "Resource": "arn:aws:ecs:region:aws-account-id:task/cluster-name/*",
            "Condition": {
                "StringEquals": {
                    "ecs:ResourceTag/environment": "development"
                }
            }
        }
    ]
}
```

With the following IAM policy example, users can't use the `execute-command` API when the container name is `production-app`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ecs:ExecuteCommand"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {                    
                    "ecs:container-name": "production-app"
                }
            }
        }
    ]
}
```

With the following IAM policy, users can only launch tasks when ECS Exec is disabled\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:RunTask",
                "ecs:StartTask",
                "ecs:CreateService",
                "ecs:UpdateService"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {                    
                    "ecs:enable-execute-command": "false"
                }
            }
        }
    ]
}
```

**Note**  
Because the `execute-command` API action contains only task and cluster resources in a request, only cluster and task tags are evaluated\.

For more information about IAM policy condition keys, see [Actions, resources, and condition keys for Amazon Elastic Container Service](/service-authorization/latest/reference/list_amazonelasticcontainerservice.html) in the *Service Authorization Reference*\.

### Limiting access to the Start Session action<a name="ecs-exec-limit-access-start-session"></a>

While starting SSM sessions on your container outside of ECS Exec is possible, this could potentially result in the sessions not being logged\. Sessions started outside of ECS Exec also count against the session quota\. We recommend limiting this access by denying the `ssm:start-session` action directly for your Amazon ECS tasks using an IAM policy\. You can deny access to all Amazon ECS tasks or to specific tasks based on the tags used\.

The following is an example IAM policy that denies access to the `ssm:start-session` action for tasks in all Regions with a specified cluster name\. You can optionally include a wildcard with the `cluster-name`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "ssm:StartSession",
            "Resource": "arn:aws:ecs:*:111122223333:task/cluster-name/*"
        }
    ]
}
```

The following is an example IAM policy that denies access to the `ssm:start-session` action on resources in all Regions tagged with tag key `Task-Tag-Key` and tag value `Exec-Task`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "ssm:StartSession",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Task-Tag-Key": "Exec-Task"
                }
            }
        }
    ]
}
```

## Troubleshooting issues with ECS Exec<a name="ecs-exec-troubleshooting"></a>

The following are troubleshooting notes to help diagnose why you may be getting an error when using ECS Exec\.

### Verify using the Amazon ECS Exec Checker<a name="ecs-exec-troubleshooting-checker"></a>

The Amazon ECS Exec Checker script provides a way to verify and validate that your Amazon ECS cluster and task have met the prerequisites for using the ECS Exec feature\. The Exec Checker script verifies both your AWS CLI environment and cluster and tasks are ready for ECS Exec, by calling various APIs on your behalf\. The tool requires the latest version of the AWS CLI and that the `jq` is available\. For more information, see [Amazon ECS Exec Checker](https://github.com/aws-containers/amazon-ecs-exec-checker) on GitHub\.

### Error when calling `execute-command`<a name="ecs-exec-troubleshooting-general"></a>

If a `The execute command failed` error occurs, the following are possible causes\.
+ The task does not have the required permissions\. Verify that the task definition used to launch your task has a task IAM role defined and that the role has the required permissions\. For more information, see [IAM permissions required for ECS Exec](#ecs-exec-required-iam-permissions)\.
+ The SSM agent is not installed or is not running
+  There is an interface Amazon VPC endpoint for Amazon ECS, but there is not one for for Systems Manager Session Manager