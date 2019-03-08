# Troubleshooting IAM Roles for Tasks<a name="troubleshoot-task-iam-roles"></a>

If you are having trouble configuring IAM roles for tasks in your cluster, you can try this known good configuration to help debug your own configuration\.

The following procedure helps you to:
+ Create a CloudWatch Logs log group to store your test logs\.
+ Create a task IAM role that has full Amazon ECS permissions\.
+ Register a task definition with a known good AWS CLI configuration that is compatible with IAM roles for tasks\.
+ Run a task from that task definition to test your container instance support for IAM roles for tasks\.
+ View the container logs from that task in CloudWatch Logs to verify that it works\.

**To test IAM roles for tasks with a known good configuration**

1. Create a CloudWatch Logs log group called `ecs-tasks`\.

   1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   1. In the left navigation pane, choose **Logs**, **Actions**, **Create log group**\.

   1. For **Log Group Name**, enter `ecs-tasks` and choose **Create log group**\.

1. Create an IAM role for your task to use\.

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane, choose **Roles**, **Create role**\. 

   1. For **Select type of trusted entity**, choose **Elastic Container Service**\.

   1. For **Select your use case**, choose **Elastic Container Service Task**, **Next: Permissions**\.

   1. On the **Attached permissions policy** page, choose **AmazonEC2ContainerServiceFullAccess**, **Next: Review**\.

   1. On the **Review** page, for **Role name**, enter `ECS-task-full-access` and choose **Create role**\.

1. Register a task definition that uses your new role\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. In the navigation pane, choose **Task Definitions**\.

   1. On the **Task Definitions** page, choose **Create new Task Definition**\.

   1. On the **Select launch type compatibility** page, choose **EC2**, **Next step**\.

   1. Scroll to the bottom of the page and choose **Configure via JSON**\.

   1. Paste the sample task definition JSON below into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.
**Note**  
Replace the `awslogs-region` value with the region in which you created your CloudWatch Logs log group\.

      ```
      {
          "taskRoleArn": "ECS-task-full-access",
          "containerDefinitions": [
              {
                  "memory": 128,
                  "essential": true,
                  "name": "amazonlinux",
                  "image": "amazonlinux",
                  "entryPoint": [
                      "/bin/bash",
                      "-c"
                  ],
                  "command": [
                      "yum install -y aws-cli; aws ecs list-tasks --region us-west-2"
                  ],
                  "logConfiguration": {
                      "logDriver": "awslogs",
                      "options": {
                          "awslogs-group": "ecs-tasks",
                          "awslogs-region": "us-west-2",
                          "awslogs-stream-prefix": "iam-role-test"
                      }
                  }
              }
          ],
          "family": "iam-role-test",
          "requiresCompatibilities": [
              "EC2"
          ],
          "volumes": [],
          "placementConstraints": [],
          "networkMode": null,
          "memory": null,
          "cpu": null
      }
      ```

   1. Verify your information and choose **Create**\.

1. Run a task from your task definition\.

   1. On the **Task Definition: iam\-role\-test** registration confirmation page, choose **Actions**, **Run Task**\.

   1. On the **Run Task** page, choose the **EC2** launch type, a cluster, and then choose **Run Task** to run your task\.

1. View the container logs in the CloudWatch Logs console\.

   1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   1. In the left navigation pane, choose **Logs**\.

   1. Select the `ecs-tasks` log group\.

   1. Select the most recent log stream\.

   1. Scroll down to view the last lines of the log stream\. You should see the output of the aws ecs list\-tasks command\.

      ```
      {
          "taskArns": [
              "arn:aws:ecs:us-east-1:aws_account_id:task/d48feb62-46e2-4cbc-a36b-e0400b993d1d"
          ]
      }
      ```

      If you receive an "`Unable to locate credentials`" error, then the following are possible causes\.
      + The IAM roles for tasks feature is not enabled on your container instances\. For more information, see [Enabling Task IAM Roles on your Container Instances](task-iam-roles.md#enable_task_iam_roles)\.
      + The credential URL is being throttled\. You can use the `ECS_TASK_METADATA_RPS_LIMIT` container agent parameter to configure the throttle limits\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.