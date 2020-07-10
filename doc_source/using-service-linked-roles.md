# Service\-Linked Role for Amazon ECS<a name="using-service-linked-roles"></a>

Amazon Elastic Container Service uses a service\-linked role for the permissions it requires to call other AWS services on your behalf\. For more information, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

Prior to the introduction of a service\-linked role for Amazon ECS, you were required to create an IAM role for your Amazon ECS services which granted Amazon ECS the permission it needed\. This role is no longer required, however it is available if needed\. For more information, see [Legacy IAM Roles for Amazon ECS](ecs-legacy-iam-roles.md)\.

## Permissions Granted by the Service\-Linked Role<a name="service-linked-role-permissions"></a>

Amazon ECS uses the service\-linked role named **AWSServiceRoleForECS** to enable Amazon ECS to call AWS APIs on your behalf\.

The **AWSServiceRoleForECS** service\-linked role trusts the `ecs.amazonaws.com` service principal to assume the role\.

The role permissions policy allows Amazon ECS to complete the following actions on resources\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ECSTaskManagement",
            "Effect": "Allow",
            "Action": [
                "ec2:AttachNetworkInterface",
                "ec2:CreateNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:DeleteNetworkInterface",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:Describe*",
                "ec2:DetachNetworkInterface",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:DeregisterTargets",
                "elasticloadbalancing:Describe*",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:RegisterTargets",
                "route53:ChangeResourceRecordSets",
                "route53:CreateHealthCheck",
                "route53:DeleteHealthCheck",
                "route53:Get*",
                "route53:List*",
                "route53:UpdateHealthCheck",
                "servicediscovery:DeregisterInstance",
                "servicediscovery:Get*",
                "servicediscovery:List*",
                "servicediscovery:RegisterInstance",
                "servicediscovery:UpdateInstanceCustomHealthStatus"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AutoScaling",
            "Effect": "Allow",
            "Action": [
                "autoscaling:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AutoScalingManagement",
            "Effect": "Allow",
            "Action": [
                "autoscaling:DeletePolicy",
                "autoscaling:PutScalingPolicy",
                "autoscaling:SetInstanceProtection",
                "autoscaling:UpdateAutoScalingGroup"
            ],
            "Resource": "*",
            "Condition": {
                "Null": {
                    "autoscaling:ResourceTag/AmazonECSManaged": "false"
                }
            }
        },
        {
            "Sid": "AutoScalingPlanManagement",
            "Effect": "Allow",
            "Action": [
                "autoscaling-plans:CreateScalingPlan",
                "autoscaling-plans:DeleteScalingPlan",
                "autoscaling-plans:DescribeScalingPlans"
            ],
            "Resource": "*"
        },
        {
            "Sid": "CWAlarmManagement",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "arn:aws:cloudwatch:*:*:alarm:*"
        },
        {
            "Sid": "ECSTagging",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags"
            ],
            "Resource": "arn:aws:ec2:*:*:network-interface/*"
        },
        {
            "Sid": "CWLogGroupManagement",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:DescribeLogGroups",
                "logs:PutRetentionPolicy"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/ecs/*"
        },
        {
            "Sid": "CWLogStreamManagement",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/ecs/*:log-stream:*"
        }
    ]
}
```

## Create the Service\-Linked Role<a name="create-service-linked-role"></a>

Under most circumstances, you don't need to manually create the service\-linked role\. For example, when you create a new cluster \(for example, with the Amazon ECS first\-run experience, the cluster creation wizard, or the AWS CLI or SDKs\), or create or update a service in the AWS Management Console, Amazon ECS creates the service\-linked role for you, if it does not already exist\.

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\.

**To allow an IAM entity to create the AWSServiceRoleForECS service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to create the service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:CreateServiceLinkedRole"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "ecs.amazonaws.com"}}
}
```

### Creating a Service\-Linked Role in IAM \(AWS CLI\)<a name="create-service-linked-role-iam-cli"></a>

You can use IAM commands from the AWS Command Line Interface to create a service\-linked role with the trust policy and inline policies that the service needs to assume the role\.

**To create a service\-linked role \(CLI\)**

Use the following command:

```
$ aws iam create-service-linked-role --aws-service-name ecs.amazonaws.com
```

## Edit the Service\-Linked Role<a name="edit-service-linked-role"></a>

Amazon ECS does not allow you to edit the AWSServiceRoleForECS service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. You can, however, edit the description of the role\. For more information, see [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\.

**To allow an IAM entity to edit the description of the AWSServiceRoleForECS service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to edit the description of a service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:UpdateRoleDescription"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "ecs.amazonaws.com"}}
}
```

## Delete the Service\-Linked Role<a name="delete-service-linked-role"></a>

If you no longer use Amazon ECS, we recommend that you delete the service\-linked role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must delete all Amazon ECS clusters in all regions before you can delete the service\-linked role\.

**To allow an IAM entity to delete the AWSServiceRoleForECS service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to delete a service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:DeleteServiceLinkedRole",
        "iam:GetServiceLinkedRoleDeletionStatus"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "ecs.amazonaws.com"}}
}
```

### Cleaning up a Service\-Linked Role<a name="service-linked-role-review-before-delete"></a>

Before you can use IAM to delete a service\-linked role, you must first confirm that the role has no active sessions and delete all Amazon ECS clusters in all AWS Regions\.

**To check whether the service\-linked role has an active session**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and choose the AWSServiceRoleForECS name \(not the check box\)\.

1. On the **Summary** page, choose **Access Advisor** and review recent activity for the service\-linked role\.
**Note**  
If you are unsure whether Amazon ECS is using the AWSServiceRoleForECS role, you can try to delete the role\. If the service is using the role, then the deletion fails and you can view the regions where the role is being used\. If the role is being used, then you must wait for the session to end before you can delete the role\. You cannot revoke the session for a service\-linked role\. 

**To remove Amazon ECS resources used by the AWSServiceRoleForECS service\-linked role**

You must delete all Amazon ECS clusters in all AWS Regions before you can delete the AWSServiceRoleForECS role\.

1. Scale all Amazon ECS services down to a desired count of 0 in all regions, and then delete the services\. For more information, see [Updating a service](update-service.md) and [Deleting a service](delete-service.md)\.

1. Force deregister all container instances from all clusters in all regions\. For more information, see [Deregister a container instance](deregister_container_instance.md)\.

1. Delete all Amazon ECS clusters in all regions\. For more information, see [Deleting a cluster](delete_cluster.md)\.

### Deleting a Service\-Linked Role in IAM \(Console\)<a name="delete-service-linked-role-iam-console"></a>

You can use the IAM console to delete a service\-linked role\.

**To delete a service\-linked role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then select the check box next to AWSServiceRoleForECS, not the name or row itself\. 

1. Choose **Delete role**\.

1. In the confirmation dialog box, review the service last accessed data, which shows when each of the selected roles last accessed an AWS service\. This helps you to confirm whether the role is currently active\. If you want to proceed, choose **Yes, Delete** to submit the service\-linked role for deletion\.

1. Watch the IAM console notifications to monitor the progress of the service\-linked role deletion\. Because the IAM service\-linked role deletion is asynchronous, after you submit the role for deletion, the deletion task can succeed or fail\. 
   + If the task succeeds, then the role is removed from the list and a notification of success appears at the top of the page\.
   + If the task fails, you can choose **View details** or **View Resources** from the notifications to learn why the deletion failed\. If the deletion fails because the role is using the service's resources, then the notification includes a list of resources, if the service returns that information\. You can then [clean up the resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-review-before-delete) and submit the deletion again\.
**Note**  
You might have to repeat this process several times, depending on the information that the service returns\. For example, your service\-linked role might use six resources and your service might return information about five of them\. If you clean up the five resources and submit the role for deletion again, the deletion fails and the service reports the one remaining resource\. A service might return all of the resources, a few of them, or it might not report any resources\.
   + If the task fails and the notification does not include a list of resources, then the service might not return that information\. To learn how to clean up the resources for that service, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. Find your service in the table, and choose the **Yes** link to view the service\-linked role documentation for that service\.

### Deleting a Service\-Linked Role in IAM \(AWS CLI\)<a name="delete-service-linked-role-iam-cli"></a>

You can use IAM commands from the AWS Command Line Interface to delete a service\-linked role\.

**To delete a service\-linked role \(CLI\)**

1. Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `deletion-task-id` from the response to check the status of the deletion task\. Enter the following command to submit a service\-linked role deletion request:

   ```
   $ aws iam delete-service-linked-role --role-name AWSServiceRoleForECS+OPTIONAL-SUFFIX
   ```

1. Use the following command to check the status of the deletion task:

   ```
   $ aws iam get-service-linked-role-deletion-status --deletion-task-id deletion-task-id
   ```

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\. If the deletion fails because the role is using the service's resources, then the notification includes a list of resources, if the service returns that information\. You can then [clean up the resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-review-before-delete) and submit the deletion again\.
**Note**  
You might have to repeat this process several times, depending on the information that the service returns\. For example, your service\-linked role might use six resources and your service might return information about five of them\. If you clean up the five resources and submit the role for deletion again, the deletion fails and the service reports the one remaining resource\. A service might return all of the resources, a few of them, or it might not report any resources\. To learn how to clean up the resources for a service that does not report any resources, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. Find your service in the table, and choose the **Yes** link to view the service\-linked role documentation for that service\.

### Deleting a Service\-Linked Role in IAM \(AWSAPI\)<a name="delete-service-linked-role-iam-api"></a>

You can use the IAM API to delete a service\-linked role\.

**To delete a service\-linked role \(API\)**

1. To submit a deletion request for a service\-linked roll, call [DeleteServiceLinkedRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_DeleteServiceLinkedRole.html)\. In the request, specify the AWSServiceRoleForECS role name\.

   Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `DeletionTaskId` from the response to check the status of the deletion task\.

1. To check the status of the deletion, call [GetServiceLinkedRoleDeletionStatus](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetServiceLinkedRoleDeletionStatus.html)\. In the request, specify the `DeletionTaskId`\.

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\. If the deletion fails because the role is using the service's resources, then the notification includes a list of resources, if the service returns that information\. You can then [clean up the resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-review-before-delete) and submit the deletion again\.
**Note**  
You might have to repeat this process several times, depending on the information that the service returns\. For example, your service\-linked role might use six resources and your service might return information about five of them\. If you clean up the five resources and submit the role for deletion again, the deletion fails and the service reports the one remaining resource\. A service might return all of the resources, a few of them, or it might not report any resources\. To learn how to clean up the resources for a service that does not report any resources, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. Find your service in the table, and choose the **Yes** link to view the service\-linked role documentation for that service\.