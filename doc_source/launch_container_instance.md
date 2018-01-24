# Launching an Amazon ECS Container Instance<a name="launch_container_instance"></a>

You can launch an Amazon ECS container instance using the AWS Management Console, as described in this topic\. Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\. After you've launched your instance, you can use it to run tasks\.

**To launch a container instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the region to use\.

1. From the console dashboard, choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, choose **Community AMIs**\.

1. Choose an AMI for your container instance\. You can choose the Amazon ECS\-optimized AMI, or another operating system, such as CoreOS or Ubuntu\. If you do not choose the Amazon ECS\-optimized AMI, you must follow the procedures in [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\.
**Note**  
For Amazon ECS\-specific CoreOS installation instructions, see [https://coreos\.com/docs/running\-coreos/cloud\-providers/ecs/](https://coreos.com/docs/running-coreos/cloud-providers/ecs/)\.

   To use the Amazon ECS\-optimized AMI, type **amazon\-ecs\-optimized** in the **Search community AMIs** field and press the **Enter** key\. Choose **Select** next to the **amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized** AMI\. 

   The current Amazon ECS–optimized AMI IDs by region are listed below for reference\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_container_instance.html)

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. The `t2.micro` instance type is selected by default\. The instance type that you select determines the resources available for your tasks to run on\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, configure the following fields accordingly\.

   1. Set the **Number of instances** field depending on how many container instances you want to add to your cluster\.

   1. \(Optional\) If you want to use Spot Instances, set the **Purchasing option** field by selecting the checkbox next to **Request Spot Instances**\. You will also need to set the other fields related to Spot Instances\. See [Spot Instance Requests](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html) for more details\.
**Note**  
If you are using Spot Instances and see a `Not available` message, you may need to choose a different instance type\.

   1. For **Network**, choose the VPC to launch your container instance into\.

   1. For **Subnet**, choose a subnet to use, or keep the default option to choose the default subnet in any Availability Zone\.

   1. Set the **Auto\-assign Public IP** field depending on whether you want your instance to be accessible from the public Internet\. If your instance should be accessible from the Internet, verify that the **Auto\-assign Public IP** field is set to **Enable**\. If your instance should not be accessible from the Internet, set this field to **Disable**\.
**Note**  
Container instances need external network access to communicate with the Amazon ECS service endpoint, so if your container instances do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) in the *Amazon VPC User Guide* and [HTTP Proxy Configuration](http_proxy_config.md) in this guide\. For help creating a VPC, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)

   1. Select the `ecsInstanceRole` **IAM role** value that you created for your container instances in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
**Important**  
If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent cannot connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. \(Optional\) Configure your Amazon ECS container instance with user data, such as the agent environment variables from [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\. Amazon EC2 user data scripts are executed only one time, when the instance is first launched\.

      By default, your container instance launches into your default cluster\. To launch into a non\-default cluster, choose the **Advanced Details** list\. Then, paste the following script into the **User data** field, replacing *your\_cluster\_name* with the name of your cluster\.

      ```
      #!/bin/bash
      echo ECS_CLUSTER=your_cluster_name >> /etc/ecs/ecs.config
      ```

      Or, if you have an `ecs.config` file in Amazon S3 and have enabled Amazon S3 read\-only access to your container instance role, choose the **Advanced Details** list\. Then, paste the following script into the **User data** field, replacing *your\_bucket\_name* with the name of your bucket to install the AWS CLI and write your configuration file at launch time\. 
**Note**  
For more information about this configuration, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3)\.

      ```
      #!/bin/bash
      yum install -y aws-cli
      aws s3 cp s3://your_bucket_name/ecs.config /etc/ecs/ecs.config
      ```

      For more information, see [Bootstrapping Container Instances with Amazon EC2 User Data](bootstrap_container_instance.md)\.

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, configure the storage for your container instance\.

   If you are using an Amazon ECS\-optimized AMI before the **2015\.09\.d** version, your instance has a single volume that is shared by the operating system and Docker\.

   If you are using the **2015\.09\.d** or later Amazon ECS\-optimized AMI, your instance has two volumes configured\. The **Root** volume is for the operating system's use, and the second Amazon EBS volume \(attached to `/dev/xvdcz`\) is for Docker's use\.

   You can optionally increase or decrease the volume sizes for your instance to meet your application needs\.

1. Choose **Review and Launch**\.

1. On the **Review Instance Launch** page, under **Security Groups**, you see that the wizard created and selected a security group for you\. Instead, select the security group that you created in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) using the following steps:

   1. Choose **Edit security groups**\.

   1. On the **Configure Security Group** page, select the **Select an existing security group** option\.

   1. Select the security group you created for your container instance from the list of existing security groups, and choose **Review and Launch**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, then select the key pair that you created when getting set up\. 

   When you are ready, select the acknowledgment field, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running`, and it receives a public DNS name\. If the **Public DNS** column is hidden, choose **Show/Hide**, **Public DNS**\.