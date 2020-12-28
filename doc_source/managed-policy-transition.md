# Migrating to the `AmazonECS_FullAccess` managed policy<a name="managed-policy-transition"></a>

The `AmazonEC2ContainerServiceFullAccess` managed IAM policy is being deprecated on January 29, 2021 in response to a security finding with the `iam:passRole` permission which grants access to all resources including credentials to roles in the account\. Once the policy is deprecated, you won't be able to attach the policy to any new IAM groups, users, or roles\. Any existing groups, users, or roles that have the policy attached will be able to continue using it, however we recommend updating your IAM groups, users, or roles to use the `AmazonECS_FullAccess` managed policy instead\.

The permissions granted by the `AmazonECS_FullAccess` policy have finer detail than the `AmazonEC2ContainerServiceFullAccess` policy\. If you are currently using permissions granted by the `AmazonEC2ContainerServiceFullAccess` policy that are not present in the `AmazonECS_FullAccess` policy you can add additional permissions to an IAM group, user, or role by adding an in\-line policy statement\. For more information about each of these policies, see [Amazon ECS Managed Policies and Trust Relationships](ecs_managed_policies.md)\.

Use the following steps to determine if you have any IAM groups, users, or roles that are currently using the `AmazonEC2ContainerServiceFullAccess` managed IAM policy and then update them to detach the deprecated policy and attach the `AmazonECS_FullAccess` policy\.

**To update an IAM group, user, or role to use the AmazonECS\_FullAccess policy \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and search for and select the `AmazonEC2ContainerServiceFullAccess` policy\.

1. Choose the **Policy usage** tab which will display any IAM role currently using this policy\.

1. For each IAM role currently using the `AmazonEC2ContainerServiceFullAccess` policy, select the role and use the following steps to detach the deprecated policy and attach the `AmazonECS_FullAccess` policy\.

   1. On the **Permissions** tab, choose the **X** next to the **AmazonEC2ContainerServiceFullAccess** policy\.

   1. Choose **Add permissions**\.

   1. Choose **Attach existing policies directly**, search for and select the **AmazonECS\_FullAccess** policy, and then choose **Next: Review**\.

   1. Review the changes and then choose **Add permissions**\.

   1. Repeat these steps for each IAM group, user, or role that is using the `AmazonEC2ContainerServiceFullAccess` policy\.

**To update an IAM group, user, or role to use the `AmazonECS_FullAccess` policy \(AWS CLI\)**

1. Use the [generate\-service\-last\-accessed\-details](https://docs.aws.amazon.com/cli/latest/reference/iam/generate-service-last-accessed-details.html) command to generate a report that includes details about when the deprecated policy was last used\.

   ```
   aws iam generate-service-last-accessed-details \
        --arn arn:aws:iam::aws:policy/AmazonEC2ContainerServiceFullAccess
   ```

   Example output:

   ```
   {
       "JobId": "32bb1fb0-1ee0-b08e-3626-ae83EXAMPLE"
   }
   ```

1. Use the job ID from the previous output with the [get\-service\-last\-accessed\-details](https://docs.aws.amazon.com/cli/latest/reference/iam/get-service-last-accessed-details.html) command to retrieve the service last accessed report\. This report will display the Amazon Resource Name \(ARN\) of the IAM entities that last used the deprecated policy\.

   ```
   aws iam get-service-last-accessed-details \
         --job-id 32bb1fb0-1ee0-b08e-3626-ae83EXAMPLE
   ```

1. Use one of the following commands to detach the `AmazonEC2ContainerServiceFullAccess` policy from an IAM group, user, or role\.
   + [detach\-group\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/detach-group-policy.html)
   + [detach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/detach-role-policy.html)
   + [detach\-user\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/detach-user-policy.html)

1. Use one of the following commands to attach the `AmazonECS_FullAccess` policy to an IAM group, user, or role\.
   + [attach\-group\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-group-policy.html)
   + [attach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html)
   + [attach\-user\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-user-policy.html)