# Using service\-linked roles for Amazon ECS<a name="using-service-linked-roles"></a>

Amazon Elastic Container Service uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Amazon ECS\. The service\-linked role is predefined by Amazon ECS and includes all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Amazon ECS easier because you don’t have to manually add the necessary permissions\. Amazon ECS defines the permissions of its service\-linked roles, and unless defined otherwise, only Amazon ECS can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Amazon ECS<a name="slr-permissions"></a>

Amazon ECS uses the service\-linked role named **AWSServiceRoleForECS**\.

The AWSServiceRoleForECS service\-linked role trusts the following services to assume the role:
+ `ecs.amazonaws.com`

The role permissions policy named AmazonECSServiceRolePolicy allows Amazon ECS to complete the following actions on the specified resources:
+ Action: When using the `awsvpc` network mode for your Amazon ECS tasks, Amazon ECS manages the lifecycle of the elastic network interfaces associated with the task\. This also includes tags that Amazon ECS adds to your elastic network interfaces\.
+ Action: When using a load balancer with your Amazon ECS service, Amazon ECS manages the registration and deregistration of resources with the load balancer\.
+ Action: When using Amazon ECS service discovery, Amazon ECS manages the required Route 53 and AWS Cloud Map resources for service discovery to work\.
+ Action: When using Amazon ECS service auto scaling, Amazon ECS manages the required Auto Scaling resources\.
+ Action: Amazon ECS creates and manages CloudWatch alarms and log streams that assist in the monitoring of your Amazon ECS resources\.
+ Action: When using Amazon ECS Exec, Amazon ECS manages the permissions needed to start Amazon ECS Exec sessions to your tasks\.
+ Action: When using Amazon ECS Service Connect, Amazon ECS manages the required AWS Cloud Map resources to use the feature\.

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Amazon ECS<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you create a cluster or create or update a service in the AWS Management Console, the AWS CLI, or the AWS API, Amazon ECS creates the service\-linked role for you\. 

**Important**  
This service\-linked role can appear in your account if you completed an action in another service that uses the features supported by this role\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create a cluster or create or update a service, Amazon ECS creates the service\-linked role for you again\. 

## Editing a service\-linked role for Amazon ECS<a name="edit-slr"></a>

Amazon ECS doesn't allow you to edit the AWSServiceRoleForECS service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Amazon ECS<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

**Note**  
If the Amazon ECS service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To check whether the service\-linked role has an active session**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and choose the AWSServiceRoleForECS name \(not the check box\)\.

1. On the **Summary** page, choose **Access Advisor** and review recent activity for the service\-linked role\.
**Note**  
If you are unsure whether Amazon ECS is using the AWSServiceRoleForECS role, you can try to delete the role\. If the service is using the role, then the deletion fails and you can view the regions where the role is being used\. If the role is being used, then you must wait for the session to end before you can delete the role\. You cannot revoke the session for a service\-linked role\. 

**To remove Amazon ECS resources used by the AWSServiceRoleForECS service\-linked role**

You must delete all Amazon ECS clusters in all AWS Regions before you can delete the AWSServiceRoleForECS role\.

1. Scale all Amazon ECS services down to a desired count of 0 in all regions, and then delete the services\. For more information, see [Updating a service using the classic console](update-service.md) and [Deleting a service using the classic console](delete-service.md)\.

1. Force deregister all container instances from all clusters in all regions\. For more information, see [Deregister an Amazon EC2 backed container instance](deregister_container_instance.md)\.

1. Delete all Amazon ECS clusters in all regions\. For more information, see [Deleting a cluster using the classic console](delete_cluster.md)\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForECS service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for Amazon ECS service\-linked roles<a name="slr-regions"></a>

Amazon ECS supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.