# Tutorial: Specifying Sensitive Data Using Secrets Manager Secrets<a name="specifying-sensitive-data-tutorial"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in AWS Secrets Manager secrets and then referencing them in your container definition\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.

The following tutorial shows how to create an Secrets Manager secret, reference the secret in an Amazon ECS task definition, and then verify it worked by querying the environment variable inside a container showing the contents of the secret\.

## Prerequisites<a name="specifying-sensitive-data-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required IAM permissions to create the Secrets Manager and Amazon ECS resources described\.

## Step 1: Create an Secrets Manager Secret<a name="specifying-sensitive-data-tutorial-create-secret"></a>

You can use the Secrets Manager console to create a secret for your sensitive data\. In this tutorial we will be creating a basic secret for storing a username and password to reference later in a container\. For more information, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

The ** key/value pairs to be stored in this secret** is the environment variable value in your container at the end of the tutorial\.

Save the **Secret ARN** to reference in your task execution IAM policy and task definition in later steps\.

## Step 2: Update Your Task Execution IAM Role<a name="specifying-sensitive-data-tutorial-update-iam"></a>

In order for Amazon ECS to retrieve the sensitive data from your Secrets Manager secret, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary Secrets Manager resources\. If you have not already created your task execution IAM role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

The following steps assume you already have the task execution IAM role created and properly configured\.

**To update your task execution IAM role**

Use the IAM console to update your task execution role with the required permissions\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsTaskExecutionRole` and select it\.

1. Choose **Permissions**, **Add inline policy**\.

1. Choose the **JSON** tab and specify the following JSON text, ensuring that you specify the full ARN of the Secrets Manager secret you created in step 1\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "secretsmanager:GetSecretValue"
               ],
               "Resource": [
                   "arn:aws:secretsmanager:region:aws_account_id:secret:username_value-u9bH6K"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\. For **Name** specify `ECSSecretsTutorial`, then choose **Create policy**\.

## Step 3: Create an Amazon ECS Task Definition<a name="specifying-sensitive-data-tutorial-create-taskdef"></a>

You can use the Amazon ECS console to create a task definition that references a Secrets Manager secret\.

**To create a task definition that specifies a secret**

Use the IAM console to update your task execution role with the required permissions\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**\.

1. Choose **Create new task definition**, **Create new task definition with JSON**\.

1. In the JSON editor box, edit your JSON file,

1. On the **Select launch type compatibility** page, choose **EC2** and choose **Next step**\.

1. Choose **Configure via JSON** and enter the following task definition JSON text, ensuring that you specify the full ARN of the Secrets Manager secret you created in step 1 and the task execution IAM role you updated in step 2\. Choose **Save**\.

   ```
   {
       "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole",
       "containerDefinitions": [
           {
               "entryPoint": [
                   "sh",
                   "-c"
               ],
               "portMappings": [
                   {
                       "hostPort": 80,
                       "protocol": "tcp",
                       "containerPort": 80
                   }
               ],
               "command": [
                   "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
               ],
               "cpu": 10,
               "secrets": [
                   {
                       "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:username_value-u9bH6K",
                       "name": "username_value"
                   }
               ],
               "memory": 300,
               "image": "httpd:2.4",
               "essential": true,
               "name": "ecs-secrets-container"
           }
       ],
       "family": "ecs-secrets-tutorial"
   }
   ```

1. Review the settings and then choose **Create**\.

## Step 4: Create an Amazon ECS Cluster<a name="specifying-sensitive-data-tutorial-create-cluster"></a>

You can use the Amazon ECS console to create a cluster containing a container instance to run the task on\. If you have an existing cluster with at least one container instance registered to it with the available resources to run one instance of the task definition created for this tutorial you can skip to the next step\.

For this tutorial we will be creating a cluster with one `t2.micro` container instance using the Amazon ECS\-optimized Amazon Linux 2 AMI\.

For information about how to create a cluster for the EC2 launch type, see [Creating a cluster for the Amazon EC2 launch type using the console](create-ec2-cluster-console-v2.md)\.

## Step 5: Run an Amazon ECS Task<a name="specifying-sensitive-data-tutorial-run-task"></a>

You can use the Amazon ECS console to run a task using the task definition you created\. For this tutorial we will be running a task using the EC2 launch type, using the cluster we created in the previous step\. 

For information about how to run a task, see [Running a standalone task using the Amazon ECS console](ecs_run_task-v2.md)\.

## Step 6: Verify<a name="specifying-sensitive-data-tutorial-verify"></a>

You can verify all of the steps were completed successfully and the environment variable was created properly in your container using the following steps\.

**To verify that the environment variable was created**

1. Find the public IP or DNS address for your container instance\.

   1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

   1. In the navigation pane, choose **Clusters**, and then chosse the cluster you created\.

   1. Choose **Infrastructure**, and then choose the container instance\.

   1. Record the **Public IP** or **Public DNS** for your instance\.

1. If you are using a macOS or Linux computer, connect to your instance with the following command, substituting the path to your private key and the public address for your instance:

   ```
   $ ssh -i /path/to/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
   ```

   For more information about using a Windows computer, see [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Important**  
For more information about any issues while connecting to your instance, see [Troubleshooting Connecting to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. List the containers running on the instance\. Note the container ID for `ecs-secrets-tutorial` container\.

   ```
   docker ps
   ```

1. Connect to the `ecs-secrets-tutorial` container using the container ID from the output of the previous step\.

   ```
   docker exec -it container_ID /bin/bash
   ```

1. Use the `echo` command to print the value of the environment variable\.

   ```
   echo $username_value
   ```

   If the tutorial was successful, you should see the following output:

   ```
   password_value
   ```
**Note**  
Alternatively, you can list all environment variables in your container using the `env` \(or `printenv`\) command\.

## Step 7: Clean Up<a name="specifying-sensitive-data-tutorial-cleanup"></a>

When you are finished with this tutorial, you should clean up the associated resources to avoid incurring charges for unused resources\.

**To clean up the resources**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.

1. In the upper\-right of the page, choose **Delete Cluster**\. 

   A message is displayed when you did not delete all the resources associated with the cluster\.

1. In the confirmation box, enter **delete *cluster name***\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsTaskExecutionRole` and select it\.

1. Choose **Permissions**, then choose the **X** next to **ECSSecretsTutorial**\. Choose **Remove**\.

1. Open the Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Select the **username\_value** secret you created and choose **Actions**, **Delete secret**\.