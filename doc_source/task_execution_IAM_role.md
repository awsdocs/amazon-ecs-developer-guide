# Amazon ECS Task Execution IAM Role<a name="task_execution_IAM_role"></a>

The Amazon ECS container agent makes calls to the Amazon ECS API actions on your behalf, so it requires an IAM policy and role for the service to know that the agent belongs to you\. The following actions are covered by the task execution role:

+ Calls to Amazon ECR to pull the container image

+ Calls to CloudWatch to store container application logs

The `AmazonECSTaskExecutionRolePolicy` policy is shown below\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:GetAuthorizationToken",
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

The Amazon ECS task execution role is automatically created for you in the console first\-run experience; however, you should manually attach the managed IAM policy for tasks to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. You can use the following procedure to check and see if your account already has the Amazon ECS task execution role and to attach the managed IAM policy if needed\.<a name="procedure_check_execution_role"></a>

**To check for the `ecsTaskExecutionRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsTaskExecutionRole`\. If the role does not exist, use the procedure below to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose the **Permissions** tab\. Ensure that the **AmazonECSTaskExecutionRolePolicy** managed policy is attached to the role\. If the policy is attached, your Amazon ECS task execution role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach policy**\.

   1. In the **Filter** box, type **AmazonECSTaskExecutionRolePolicy** to narrow the available policies to attach\.

   1. Check the box to the left of the **AmazonECSTaskExecutionRolePolicy** policy and choose **Attach policy**\.

1. Choose the **Trust relationships** tab, and **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

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

**To create the `ecsTaskExecutionRole` IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. In the **Select type of trusted entity** section, choose **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task**, then choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, search for **AmazonECSTaskExecutionRolePolicy** and select the policy and choose **Next: Review**\.

1. For **Role Name**, type `ecsTaskExecutionRole` and choose **Create role**\.