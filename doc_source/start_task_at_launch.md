# Starting a Task at Container Instance Launch Time<a name="start_task_at_launch"></a>

Depending on your application architecture design, you may need to run a specific container on every container instance to deal with operations or security concerns such as monitoring, security, metrics, service discovery, or logging\.

To do this, you can configure your container instances to call the docker run command with the user data script at launch, or in some init system such as Upstart or systemd\. While this method works, it has some disadvantages because Amazon ECS has no knowledge of the container and cannot monitor the CPU, memory, ports, or any other resources used\. To ensure that Amazon ECS can properly account for all task resources, create a task definition for the container to run on your container instances\. Then, use Amazon ECS to place the task at launch time with Amazon EC2 user data\.

The Amazon EC2 user data script in the following procedure uses the Amazon ECS introspection API to identify the container instance\. Then, it uses the AWS CLI and the start\-task command to run a specified task on itself during startup\. 

**To start a task at container instance launch time**

1. If you have not done so already, create a task definition with the container you want to run on your container instance at launch by following the procedures in [Creating a Task Definition](create-task-definition.md)\.

1. Modify your `ecsInstanceRole` IAM role to add permissions for the `StartTask` API operation\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane, choose **Roles**\. 

   1. Choose the `ecsInstanceRole`\. If the role does not exist, use the procedure in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) to create the role and return to this procedure\. If the role does exist, select the role to view the attached policies\.

   1. In the **Inline Policies** section, choose **Create Role Policy**\.

   1. On the **Set Permissions** page, choose **Custom Policy**, **Select**\.

   1. For **Policy Name**, enter `StartTask`\.

   1. For **Policy Document**, copy and paste the following policy and choose **Apply Policy**\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "ecs:StartTask"
            ],
            "Resource": "*"
          }
        ]
      }
      ```

1. Launch one or more container instances by following the procedure in [Launching an Amazon ECS Container Instance](launch_container_instance.md), but in [[ERROR] BAD/MISSING LINK TEXT](launch_container_instance.md#instance-launch-user-data-step)\. Then, copy and paste the MIME multi\-part user data script below into the **User data** field\. Substitute *your\_cluster\_name* with the cluster for the container instance to register into and *my\_task\_def* with the task definition to run on the instance at launch\.
**Note**  
The MIME mult\-ipart content below uses a shell script to set configuration values and install packages\. It also uses an Upstart job to start the task after the ecs service is running and the introspection API is available\.

   ```
   Content-Type: multipart/mixed; boundary="==BOUNDARY=="
   MIME-Version: 1.0
   
   --==BOUNDARY==
   Content-Type: text/x-shellscript; charset="us-ascii"
   
   #!/bin/bash
   # Specify the cluster that the container instance should register into
   cluster=your_cluster_name
   
   # Write the cluster configuration variable to the ecs.config file
   # (add any other configuration variables here also)
   echo ECS_CLUSTER=$cluster >> /etc/ecs/ecs.config
   
   # Install the AWS CLI and the jq JSON parser
   yum install -y aws-cli jq
   
   --==BOUNDARY==
   Content-Type: text/upstart-job; charset="us-ascii"
   
   #upstart-job
   description "Amazon EC2 Container Service (start task on instance boot)"
   author "Amazon Web Services"
   start on started ecs
   
   script
   	exec 2>>/var/log/ecs/ecs-start-task.log
   	set -x
   	until curl -s http://localhost:51678/v1/metadata
   	do
   		sleep 1
   	done
   
   	# Grab the container instance ARN and AWS region from instance metadata
   	instance_arn=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F/ '{print $NF}' )
   	cluster=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .Cluster' | awk -F/ '{print $NF}' )
   	region=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F: '{print $4}')
   
   	# Specify the task definition to run at launch
   	task_definition=my_task_def
   
   	# Run the AWS CLI start-task command to start your task on this container instance
   	aws ecs start-task --cluster $cluster --task-definition $task_definition --container-instances $instance_arn --started-by $instance_arn --region $region
   end script
   --==BOUNDARY==--
   ```

1. Verify that your container instances launch into the correct cluster and that your tasks have started\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. From the navigation bar, choose the region that your cluster is in\.

   1. In the navigation pane, choose **Clusters** and select the cluster that hosts your container instances\.

   1. On the **Cluster** page, choose **Tasks**\.  
![\[ECS Instances Tab\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/1_task_per_instance.png)

      Each container instance you launched should have your task running on it, and the container instance ARN should be in the **Started By** column\.

      If you do not see your tasks, you can log in to your container instances with SSH and check the `/var/log/ecs/ecs-start-task.log` file for debugging information\.