# **Amazon Elastic Container Service Task Role**<a name="task_IAM_role"></a>

Before you can use IAM roles for tasks , Amazon ECS needs permission to make calls to the AWS APIs on your behalf\. These permissions are provided by the Amazon Elastic Container Service Task Role\.

You can create a task IAM role for each task definition that needs permission to call AWS APIs\. You simply create an IAM policy that defines which permissions your task should have, and then attach that policy to a role that uses the Amazon Elastic Container Service Task Role trust relationship policy\. For more information, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\. 

The Amazon Elastic Container Service Task Role trust relationship is shown below\.

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