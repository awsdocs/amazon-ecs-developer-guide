# Tutorial: Specifying sensitive data using Secrets Manager secrets<a name="specifying-sensitive-data-tutorial"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in AWS Secrets Manager secrets and then referencing them in your container definition\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.

The following tutorial shows how to create an Secrets Manager secret, reference the secret in an Amazon ECS task definition, and then verify it worked by querying the environment variable inside a container showing the contents of the secret\.

## Prerequisites<a name="specifying-sensitive-data-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required IAM permissions to create the Secrets Manager and Amazon ECS resources described\.

## Step 1: Create an Secrets Manager secret<a name="specifying-sensitive-data-tutorial-create-secret"></a>

You can use the Secrets Manager console to create a secret for your sensitive data\. In this tutorial we will be creating a basic secret for storing a username and password to reference later in a container\. For more information, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

**To create a basic secret**

Use Secrets Manager to create a secret for your sensitive data\.

1. Open the Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Choose **Store a new secret**\.

1. For **Select secret type**, choose **Other type of secrets**\.

1. For **Specify the key/value pairs to be stored in this secret**, choose the **Plaintext** tab and replace the existing text with the following text\. The text value you specify here will be the environment variable value in your container at the end of the tutorial\.

   ```
   password_value
   ```

1. Choose **Next**\.

1. For **Secret name**, type `username_value` and choose **Next**\. The secret name value you specify here will be the environment variable name in your container at the end of the tutorial\.

1. For **Configure automatic rotation**, leave **Disable automatic rotation** selected and choose **Next**\.

1. Review these settings, and then choose **Store** to save everything you entered as a new secret in Secrets Manager\.

1. Select the secret you just created and save the **Secret ARN** to reference in your task execution IAM policy and task definition in later steps\.

## Step 2: Update your task execution IAM role<a name="specifying-sensitive-data-tutorial-update-iam"></a>

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

## Step 3: Create an Amazon ECS task definition<a name="specifying-sensitive-data-tutorial-create-taskdef"></a>

You can use the Amazon ECS console to create a task definition that references a Secrets Manager secret\.

**To create a task definition that specifies a secret**

Use the IAM console to update your task execution role with the required permissions\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. On the **Select launch type compatibility** page, choose **EC2** and choose **Next step**\.

1. Choose **Configure via JSON** and enter the following task definition JSON text, ensuring that you specify the full ARN of the Secrets Manager secret you created in step 1 and the task execution IAM role you updated in step 2\. Choose **Save**\.
**Important**  
The value for the secret name in the task definition must match the name you specified for the secret name when the secret was created\.

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

## Step 4: Create an Amazon ECS cluster<a name="specifying-sensitive-data-tutorial-create-cluster"></a>

You can use the Amazon ECS console to create a cluster containing a container instance to run the task on\. If you have an existing cluster with at least one container instance registered to it with the available resources to run one instance of the task definition created for this tutorial you can skip to the next step\.

For this tutorial we will be creating a cluster with one `t2.micro` container instance using the Amazon ECS\-optimized Amazon Linux 2 AMI\.

**To create a cluster**

Use the Amazon ECS console to create a cluster and register one container instance to it\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region that contains both the Secrets Manager secret and the Amazon ECS task definition you created\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. For **EC2 instance type**, choose **t2\.micro**\.

1. For **Key pair**, choose a key pair to add to the container instance\.
**Important**  
A key pair is required to complete the tutorial, so if you do not already have a key pair created follow the link to the EC2 console to create one\.

1. Leave all other fields at their default values and choose **Create**\.

## Step 5: Run an Amazon ECS task<a name="specifying-sensitive-data-tutorial-run-task"></a>

You can use the Amazon ECS console to run a task using the task definition you created\. For this tutorial we will be running a task using the EC2 launch type, using the cluster we created in the previous step\.

**To run a task**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions** and select the **ecs\-secrets\-tutorial** task definition we created\.

1. Select the latest revision of the task definition and then choose **Actions**, **Run Task**\.

1. For **Launch Type**, choose **EC2**\.

1. For **Cluster**, choose the **ecs\-secrets\-tutorial** cluster we created in the previous step\. 

1. For **Task tagging configuration**, deselect **Enable ECS managed tags**\. They are unnecessary for the purposes of this tutorial\.

1. Review your task information and choose **Run Task**\.
**Note**  
If your task moves from `PENDING` to `STOPPED`, or if it displays a `PENDING` status and then disappears from the listed tasks, your task may be stopping due to an error\. For more information, see [Checking stopped tasks for errors](stopped-task-errors.md) in the troubleshooting section\.

## Step 6: Verify<a name="specifying-sensitive-data-tutorial-verify"></a>

You can verify all of the steps were completed successfully and the environment variable was created properly in your container using the following steps\.

**To verify that the environment variable was created**

1. Find the public IP or DNS address for your container instance\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the `ecs-secrets-tutorial` cluster that hosts your container instance\.

   1. On the **Cluster** page, choose **ECS Instances**\.

   1. On the **Container Instance** column, select the container instance to connect to\.

   1. On the **Container Instance** page, record the **Public IP** or **Public DNS** for your instance\.

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

## Step 7: Clean up<a name="specifying-sensitive-data-tutorial-cleanup"></a>

When you are finished with this tutorial, you should clean up the associated resources to avoid incurring charges for unused resources\.

**To clean up the resources**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the **ecs\-secrets\-tutorial cluster** you created\.

1. On the **Cluster** page, choose **Delete Cluster**\.

1. Enter the delete cluster confirmation phrase and choose **Delete**\. This will take several minutes but will clean up all of the Amazon ECS cluster resources\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsTaskExecutionRole` and select it\.

1. Choose **Permissions**, then choose the **X** next to `ECSSecretsTutorial`\. Choose **Remove** to confirm the removal of the inline policy\.

1. Open the Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Select the **username\_value** secret you created and choose **Actions**, **Delete secret**\.