# Launching an Amazon ECS Container Instance<a name="launch_container_instance"></a>

You can launch an Amazon ECS container instance using the AWS Management Console, as described in this topic\. Before you begin, be sure that you've completed the steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\. After you've launched your instance, you can use it to run tasks\.

**To launch a container instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the region to use\.

1. From the console dashboard, choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, complete the following steps:

   1. Choose **Community AMIs**\.

   1. Choose an AMI for your container instance\. You can choose an Amazon ECS\-optimized AMI, or another operating system, such as CoreOS or Ubuntu\. If you do not choose an Amazon ECS\-optimized AMI, you must follow the procedures in [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\.
**Note**  
For more information about Amazon ECS\-specific CoreOS installation instructions, see [Running CoreOS Container Linux with AWS EC2 Container Service](https://coreos.com/docs/running-coreos/cloud-providers/ecs/)\.

      For more information on the latest Amazon ECS\-optimized AMIs, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. The `t2.micro` instance type is selected by default\. The instance type that you select determines the resources available for your tasks to run on\.

   Choose **Next: Configure Instance Details** when you are done\.

1. On the **Configure Instance Details** page, complete the following steps:

   1. Set the **Number of instances** field depending on how many container instances you want to add to your cluster\.

   1. \(Optional\) To use Spot Instances, for **Purchasing option**, select the check box next to **Request Spot Instances**\. You also need to set the other fields related to Spot Instances\. For more information, see [Spot Instance Requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html)\.
**Note**  
If you are using Spot Instances and see a `Not available` message, you may need to choose a different instance type\.

   1. For **Network**, choose the VPC into which to launch your container instance\.

   1. For **Subnet**, choose a subnet to use, or keep the default option to choose the default subnet in any Availability Zone\.

   1. Set the **Auto\-assign Public IP** field depending on whether you want your instance to be accessible from the public internet\. If your instance should be accessible from the internet, verify that the **Auto\-assign Public IP** field is set to **Enable**\. If not, set this field to **Disable**\.
**Note**  
Container instances need access to communicate with the Amazon ECS service endpoint\. This can be through an interface VPC endpoint or through your container instances having public IP addresses\.  
For more information about interface VPC endpoints, see [Amazon ECS interface VPC endpoints \(AWS PrivateLink\)](vpc-endpoints.md)\.  
If you do not have an interface VPC endpoint configured and your container instances do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide* and [HTTP Proxy Configuration](http_proxy_config.md) in this guide\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

   1. Select your container instance IAM role\. This is usually named `ecsInstanceRole`\.
**Important**  
If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent cannot connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. <a name="instance-launch-user-data-step"></a>\(Optional\) Configure your Amazon ECS container instance with user data, such as the agent environment variables from [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\. Amazon EC2 user data scripts are executed only one time, when the instance is first launched\. The following are common examples of what user data is used for:
      + By default, your container instance launches into your default cluster\. To launch into a non\-default cluster, choose the **Advanced Details** list\. Then, paste the following script into the **User data** field, replacing *your\_cluster\_name* with the name of your cluster\.

        ```
        #!/bin/bash
        echo ECS_CLUSTER=your_cluster_name >> /etc/ecs/ecs.config
        ```
      + If you have an `ecs.config` file in Amazon S3 and have enabled Amazon S3 read\-only access to your container instance role, choose the **Advanced Details** list\. Then, paste the following script into the **User data** field, replacing *your\_bucket\_name* with the name of your bucket to install the AWS CLI and write your configuration file at launch time\. 
**Note**  
For more information about this configuration, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3)\.

        ```
        #!/bin/bash
        yum install -y aws-cli
        aws s3 cp s3://your_bucket_name/ecs.config /etc/ecs/ecs.config
        ```
      + Specify tags for your container instance using the `ECS_CONTAINER_INSTANCE_TAGS` configuration parameter\. This creates tags that are associated with Amazon ECS only, they cannot be listed using the Amazon EC2 API\.
**Important**  
If you launch your container instances using an Amazon EC2 Auto Scaling group, then you should use the ECS\_CONTAINER\_INSTANCE\_TAGS agent configuration parameter to add tags\. This is due to the way in which tags are added to Amazon EC2 instances that are launched using Auto Scaling groups\.

        ```
        #!/bin/bash
        cat <<'EOF' >> /etc/ecs/ecs.config
        ECS_CLUSTER=your_cluster_name
        ECS_CONTAINER_INSTANCE_TAGS={"tag_key": "tag_value"}
        EOF
        ```
      + Specify tags for your container instance and then use the `ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM` configuration parameter to propagate them from Amazon EC2 to Amazon ECS

        The following is an example of a user data script that would propagate the tags associated with a container instance, as well as register the container instance with a cluster named `your_cluster_name`:

        ```
        #!/bin/bash
        cat <<'EOF' >> /etc/ecs/ecs.config
        ECS_CLUSTER=your_cluster_name
        ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM=ec2_instance
        EOF
        ```

      For more information, see [Bootstrapping Container Instances with Amazon EC2 User Data](bootstrap_container_instance.md)\.

   1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, configure the storage for your container instance\.

   If you are using the Amazon ECS\-optimized Amazon Linux 2 AMI, your instance has a single 30 GiB volume configured, which is shared between the operating system and Docker\.

   If you are using the Amazon ECS\-optimized AMI, your instance has two volumes configured\. The **Root** volume is for the operating system's use, and the second Amazon EBS volume \(attached to `/dev/xvdcz`\) is for Docker's use\.

   You can optionally increase or decrease the volume sizes for your instance to meet your application needs\.

   When done configuring your volumes, choose **Next: Add Tags**\.

1. On the **Add Tags** page, specify tags by providing key and value combinations for the container instance\. Choose **Add another tag** to add more than one tag to your container instance\. For more information resource tags, see [Resources and tags](ecs-resource-tagging.md)\.

   Choose **Next: Configure Security Group** when you are done\.

1. On the **Configure Security Group** page, use a security group to define firewall rules for your container instance\. These rules specify which incoming network traffic is delivered to your container instance\. All other traffic is ignored\. Select or create a security group as follows, and then choose **Review and Launch**\.

1. On the **Review Instance Launch** page, under **Security Groups**, you see that the wizard created and selected a security group for you\. Instead, select the security group that you created in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) using the following steps:

   1. Choose **Edit security groups**\.

   1. On the **Configure Security Group** page, select the **Select an existing security group** option\.

   1. Select the security group you created for your container instance from the list of existing security groups, and choose **Review and Launch**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, then select the key pair that you created when getting set up\. 

   When you are ready, select the acknowledgment field, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running`, and it receives a public DNS name\. If the **Public DNS** column is hidden, choose **Show/Hide**, **Public DNS**\.