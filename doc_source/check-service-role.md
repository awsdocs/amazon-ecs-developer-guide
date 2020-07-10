# Creating the service role for your account<a name="check-service-role"></a>

Amazon ECS needs permissions to register and deregister container instances with your load balancer when tasks are created and stopped\.

In most cases, the Amazon ECS service role is automatically created for you in the Amazon ECS console first run experience\. You can use the following procedure to check and see if your account already has an Amazon ECS service role\.

**To check for the `ecsServiceRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsServiceRole`\. If the role does not exist, see [Service Scheduler IAM Role](ecs-legacy-iam-roles.md#service_IAM_role) to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose **Permissions**\.

1. In the **Managed Policies** section, ensure that the **AmazonEC2ContainerServiceRole** managed policy is attached to the role\. If the policy is attached, your Amazon ECS service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach Policy**\.

   1. For **Filter**, type **AmazonEC2ContainerServiceRole** to narrow the available policies to attach\.

   1. Select the box to the left of the **AmazonEC2ContainerServiceRole** policy and choose **Attach Policy**\.

1. Choose **Trust Relationships**, **Edit Trust Relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2008-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ecs.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```