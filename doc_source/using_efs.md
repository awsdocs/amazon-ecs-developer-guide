# Tutorial: Using Amazon EFS File Systems with Amazon ECS<a name="using_efs"></a>

Amazon Elastic File System \(Amazon EFS\) provides simple, scalable file storage for use with Amazon EC2 instances\. With Amazon EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files\. Your applications can have the storage they need, when they need it\. 

You can use Amazon EFS file systems with Amazon ECS to export file system data across your fleet of container instances\. That way, your tasks have access to the same persistent storage, no matter the instance on which they land\. However, you must configure your container instance AMI to mount the Amazon EFS file system before the Docker daemon starts\. Also, your task definitions must reference volume mounts on the container instance to use the file system\. The following sections help you get started using Amazon EFS with Amazon ECS\.

**Note**  
Amazon EFS is not available in all regions\. For more information about which regions support Amazon EFS, see [Amazon Elastic File System](https://docs.aws.amazon.com/general/latest/gr/rande.html#elasticfilesystem-region) in the [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) section of the *AWS General Reference*\.

**Topics**
+ [Step 1: Gather Cluster Information](#efs-cluster-info)
+ [Step 2: Create a Security Group for an Amazon EFS File System](#efs-security-group)
+ [Step 3: Create an Amazon EFS File System](#efs-create)
+ [Step 4: Configure Container Instances](#efs-config-instance)
+ [Step 5: Create a Task Definition to Use the Amazon EFS File System](#efs-task-def)
+ [Step 6: Add Content to the Amazon EFS File System](#efs-add-content)
+ [Step 7: Run a Task and View the Results](#efs-run-task)

## Step 1: Gather Cluster Information<a name="efs-cluster-info"></a>

Before you can create all of the required resources to use Amazon EFS with your Amazon ECS cluster, gather some basic information about the cluster, such as the VPC it is hosted inside of, and the security group that it uses\.

**To gather the VPC and security group IDs for a cluster**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select one of the container instances from your cluster and view the **Description** tab of the instance details\. If you created your cluster with the Amazon ECS first\-run or cluster creation wizards, the cluster name should be part of the EC2 instance name\. For example, a cluster named `default` has this EC2 instance name: `ECS Instance - EC2ContainerService-default`\.

1. Record the **VPC ID** value for your container instance\. Later, you create a security group and an Amazon EFS file system in this VPC\.

1. Open the security group to view its details\.

1. Record the **Group ID**\. Later, you allow inbound traffic from this security group to your Amazon EFS file system\.

## Step 2: Create a Security Group for an Amazon EFS File System<a name="efs-security-group"></a>

In this section, you create a security group for your Amazon EFS file system that allows inbound access from your container instances\.

**To create a security group for an Amazon EFS file system**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation pane, choose **Security Groups**, **Create Security Group**\.

1. For **Security group name**, enter a unique name for your security group\. For example, `EFS-access-for-sg-dc025fa2`\.

1. For **Description**, enter a description for your security group\.

1. For **VPC**, choose the VPC that you identified earlier for your cluster\.

1. Choose **Inbound**, **Add rule**\.

1. For **Type**, choose **NFS**\.

1. For **Source**, choose **Custom** and then enter the security group ID that you identified earlier for your cluster\.

1. Choose **Create**\.

## Step 3: Create an Amazon EFS File System<a name="efs-create"></a>

Before you can use Amazon EFS with your container instances, you must create an Amazon EFS file system\.

**To create an Amazon EFS file system for Amazon ECS container instances**

1. Open the Amazon Elastic File System console at [https://console\.aws\.amazon\.com/efs/](https://console.aws.amazon.com/efs/)\.
**Note**  
Amazon EFS is not available in all regions\. For more information about which regions support Amazon EFS, see [Amazon Elastic File System](https://docs.aws.amazon.com/general/latest/gr/rande.html#elasticfilesystem-region) in the [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) section of the *AWS General Reference*\.

1. Choose **Create file system**\.

1. On the **Configure file system access** page, choose the VPC that your  container instances are hosted in\. By default, each subnet in the specified VPC receives a mount target that uses the default security group for that VPC\.
**Note**  
Your Amazon EFS file system and your container instances must be in the same VPC\.

1. Under **Create mount targets**, for **Security groups**, add the security group that you created in the previous section\. Choose **Next step**\.

1. Configure optional settings and then choose **Next step** to proceed\.

   1. \(Optional\) Add tags for your file system\. For example, you could specify a unique name for the file system by entering that name in the **Value** column next to the **Name** key\.

   1. \(Optional\) Enable lifecycle management to save money on infrequently accessed storage\. For more information, see [EFS Lifecycle Management](https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html) in the *Amazon Elastic File System User Guide*\.

   1. Choose a throughput mode for your file system\.
**Note**  
**Bursting** is the default, and it is recommended for most file systems\.

   1. Choose a performance mode for your file system\.
**Note**  
**General Purpose** is the default, and it is recommended for most file systems\.

   1. \(Optional\) Enable encryption\. Select the check box to enable encryption of your Amazon EFS file system at rest\.

1. Review your file system options and choose **Create File System**\.

## Step 4: Configure Container Instances<a name="efs-config-instance"></a>

After you've created your Amazon EFS file system in the same VPC as your container instances, you must configure the container instances to access and use the file system\.

**Configure a running container instance to use an Amazon EFS file system**

1. Log in to the container instance via SSH\. For more information, see [Connect to Your Container Instance](instance-connect.md)\.

1. Create a mount point for your Amazon EFS file system\. For example, `/efs`\.

   ```
   sudo mkdir /mnt/efs
   ```

1. Install the `amazon-efs-utils` client software on your container instance\.
   + For Amazon Linux:

     ```
     sudo yum install -y amazon-efs-utils
     ```
   + For other Linux distributions, see [Installing the amazon\-efs\-utils Package on Other Linux Distributions](https://docs.aws.amazon.com/efs/latest/ug/using-amazon-efs-utils.html#installing-other-distro) in the *Amazon Elastic File System User Guide*\.

1. Make a backup of the `/etc/fstab` file\.

   ```
   sudo cp /etc/fstab /etc/fstab.bak
   ```

1. Update the `/etc/fstab` file to automatically mount the file system at boot\.

   ```
   echo 'fs-12345678:/ /mnt/efs efs defaults,_netdev 0 0' | sudo tee -a /etc/fstab
   ```

1. Reload the file system table to verify that your mounts are working properly\.

   ```
   sudo mount -a
   ```
**Note**  
If you receive an error while running the above command, examine your `/etc/fstab` file for problems\. If necessary, restore it with the backup that you created earlier\.

1. Validate that the file system is mounted correctly with the following command\. You should see a file system entry that matches your Amazon EFS file system\. If not, see [Troubleshooting Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/troubleshooting.html) in the *Amazon Elastic File System User Guide*\.

   ```
   mount | grep efs
   ```

**Bootstrap an instance to use Amazon EFS with user data**

You can use an Amazon EC2 user data script to bootstrap an Amazon ECS–optimized AMI at boot\. For more information, see [Bootstrapping Container Instances with Amazon EC2 User Data](bootstrap_container_instance.md)\.

1. Follow the container instance launch instructions at [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

1. On [Step 7](launch_container_instance.md#instance-launch-user-data-step), pass the following user data to configure your instance\. If you are not using the `default` cluster, be sure to replace the `ECS_CLUSTER=default` line in the configuration file to specify your own cluster name\.

   ```
   Content-Type: multipart/mixed; boundary="==BOUNDARY=="
   MIME-Version: 1.0
   
   --==BOUNDARY==
   Content-Type: text/cloud-boothook; charset="us-ascii"
   
   # Install amazon-efs-utils
   cloud-init-per once yum_update yum update -y
   cloud-init-per once install_amazon-efs-utils yum install -y amazon-efs-utils
   
   # Create /mnt/efs folder
   cloud-init-per once mkdir_efs mkdir /mnt/efs
   
   # Mount /mnt/efs
   cloud-init-per once mount_efs echo -e 'fs-12345678:/ /mnt/efs efs defaults,_netdev 0 0' >> /etc/fstab
   mount -a
   
   --==BOUNDARY==
   Content-Type: text/x-shellscript; charset="us-ascii"
   
   #!/bin/bash
   # Set any ECS agent configuration options
   echo "ECS_CLUSTER=default" >> /etc/ecs/ecs.config
   
   --==BOUNDARY==--
   ```

## Step 5: Create a Task Definition to Use the Amazon EFS File System<a name="efs-task-def"></a>

Because the file system is mounted on the host container instance, you must create a volume mount in your Amazon ECS task definition that allows your containers to access the file system\. For more information, see [Using Data Volumes in Tasks](using_data_volumes.md)\.

The following task definition creates a data volume called `efs-html` at `/efs/html` on the host container instance Amazon EFS file system\. The `nginx` container mounts the host data volume at the NGINX root, `/usr/share/nginx/html`\.

```
{
  "containerDefinitions": [
    {
      "memory": 128,
      "portMappings": [
        {
          "hostPort": 80,
          "containerPort": 80,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "mountPoints": [
        {
          "containerPath": "/usr/share/nginx/html",
          "sourceVolume": "efs-html"
        }
      ],
      "name": "nginx",
      "image": "nginx"
    }
  ],
  "volumes": [
    {
      "host": {
        "sourcePath": "/efs/html"
      },
      "name": "efs-html"
    }
  ],
  "family": "nginx-efs"
}
```

You can save this task definition to a file called `nginx-efs.json` and register it to use in your own clusters with the following AWS CLI command\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

```
aws ecs register-task-definition --cli-input-json file://nginx-efs.json
```

## Step 6: Add Content to the Amazon EFS File System<a name="efs-add-content"></a>

For the NGINX example task, you created a directory at `/efs/html` on the container instance to host the web content\. Before the NGINX containers can serve any web content, you must add the content to the file system\. In this section, you log in to a container instance and add an `index.html` file\.

**To add content to the file system**

1. Connect using SSH to one of your container instances that is using the Amazon EFS file system\. For more information, see [Connect to Your Container Instance](instance-connect.md)\.

1. Write a simple HTML file by copying and pasting the following block of text into a terminal\.

   ```
   sudo bash -c "cat >/efs/html/index.html" <<'EOF'
   <html>
       <body>
           <h1>It Works!</h1>
           <p>You are using an Amazon EFS file system for persistent container storage.</p>
       </body>
   </html>
   EOF
   ```

## Step 7: Run a Task and View the Results<a name="efs-run-task"></a>

Now that your Amazon EFS file system is available on your container instances and there is web content for the NGINX containers to serve, you can run a task using the task definition that you created earlier\. The NGINX web servers serve your simple HTML page\. If you update the content in your Amazon EFS file system, those changes are propagated to any containers that have also mounted that file system\.

**To run a task and view the results**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster that you have configured to use Amazon EFS\.

1. Choose  **Tasks**, **Run new task** \.

1. For **Task Definition**, choose the `nginx-efs` taskjob definition that you created earlier and choose **Run Task**\. For more information on the other options in the run task workflow, see [Running Tasks](ecs_run_task.md)\.

1. Below the **Tasks** tab, choose the task that you just ran\.

1. Expand the container name at the bottom of the page, and choose the IP address that is associated with the container\. Your browser should open a new tab with the following message:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/it_works.png)
**Note**  
If you do not see the message, make sure that the security group for your container instances allows inbound network traffic on port 80\.