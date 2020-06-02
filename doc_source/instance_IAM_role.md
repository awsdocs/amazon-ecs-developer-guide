# Amazon ECS Container Instance IAM Role<a name="instance_IAM_role"></a>

The Amazon ECS container agent makes calls to the Amazon ECS API on your behalf\. Container instances that run the agent require an IAM policy and role for the service to know that the agent belongs to you\. Before you can launch container instances and register them into a cluster, you must create an IAM role for those container instances to use when they are launched\. This requirement applies to container instances launched with the Amazon ECS\-optimized AMI provided by Amazon, or with any other instances that you intend to run the agent on\. This IAM role only applies if you are using the EC2 launch type\.

**Important**  
Containers that are running on your container instances have access to all of the permissions that are supplied to the container instance role through [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\. We recommend that you limit the permissions in your container instance role to the minimal list of permissions provided in the managed `AmazonEC2ContainerServiceforEC2Role` policy shown below\. If the containers in your tasks need extra permissions that are not listed here, we recommend providing those tasks with their own IAM roles\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.  
You can prevent containers on the `docker0` bridge from accessing the permissions supplied to the container instance role \(while still allowing the permissions that are provided by [IAM Roles for Tasks](task-iam-roles.md)\) by running the following iptables command on your container instances; however, containers will not be able to query instance metadata with this rule in effect\. Note that this command assumes the default Docker bridge configuration and it will not work for containers that use the `host` network mode\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.  

```
sudo yum install -y iptables-services; sudo iptables --insert FORWARD 1 --in-interface docker+ --destination 169.254.169.254/32 --jump DROP
```
You must save this iptables rule on your container instance for it to survive a reboot\. For the Amazon ECS\-optimized AMI, use the following command\. For other operating systems, consult the documentation for that OS\.  
For the Amazon ECS\-optimized Amazon Linux 2 AMI:  

  ```
  sudo iptables-save | sudo tee /etc/sysconfig/iptables && sudo systemctl enable --now iptables
  ```
For the Amazon ECS\-optimized Amazon Linux AMI:  

  ```
  sudo service iptables save
  ```

The `AmazonEC2ContainerServiceforEC2Role` policy is shown below\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeTags",
                "ecs:CreateCluster",
                "ecs:DeregisterContainerInstance",
                "ecs:DiscoverPollEndpoint",
                "ecs:Poll",
                "ecs:RegisterContainerInstance",
                "ecs:StartTelemetrySession",
                "ecs:UpdateContainerInstancesState",
                "ecs:Submit*",
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

**Note**  
The `ecs:CreateCluster` line in the above policy is optional, provided that the cluster you intend to register your container instance into already exists\. If the cluster does not already exist, the agent must have permission to create it, or you can create the cluster with the create\-cluster command prior to launching your container instance\.  
If you omit the `ecs:CreateCluster` line, the Amazon ECS container agent can not create clusters, including the default cluster\.

The `ecs:Poll` line in the above policy is used to grant the agent permission to connect with the Amazon ECS service to report status and get commands\.

The Amazon ECS instance role is automatically created for you in the console first\-run experience\. However, you should manually attach the managed IAM policy for container instances to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. Use the following procedure to check and see if your account already has the Amazon ECS instance role and to attach the managed IAM policy if needed\.<a name="procedure_check_instance_role"></a>

**To check for the `ecsInstanceRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsInstanceRole`\. If the role does not exist, use the procedure in the next section to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose the **Permissions** tab\.

1. In the **Managed Policies** section, ensure that the **AmazonEC2ContainerServiceforEC2Role** managed policy is attached to the role\. If the policy is attached, your Amazon ECS instance role is properly configured\. If not, follow the substeps below to attach the policy\.
**Important**  
The **AmazonEC2ContainerServiceforEC2Role** managed policy should be attached to the container instance IAM role, otherwise you will receive an error using the AWS Management Console to create clusters\.

   1. Choose **Attach Policy**\.

   1. In the **Filter** box, type **AmazonEC2ContainerServiceforEC2Role** to narrow the available policies to attach\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceforEC2Role** policy and choose **Attach Policy**\.

1. Choose the **Trust Relationships** tab, and **Edit Trust Relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2008-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ec2.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To create the `ecsInstanceRole` IAM role for your container instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Elastic Container Service**\.

1. Choose the **EC2 Role for Elastic Container Service** use case and then **Next: Permissions**\.

1. In the **Attached permissions policy** section, select **AmazonEC2ContainerServiceforEC2Role** and then choose **Next: Review**\.
**Important**  
The **AmazonEC2ContainerServiceforEC2Role** managed policy should be attached to the container instance IAM role, otherwise you will receive an error using the AWS Management Console to create clusters\.

1. For **Role name**, type `ecsInstanceRole` and optionally you can enter a description\.

1. Review your role information and then choose **Create role** to finish\. 

## Adding Amazon S3 Read\-only Access to your Container Instance Role<a name="container-instance-role-s3"></a>

Storing configuration information in a private bucket in Amazon S3 and granting read\-only access to your container instance IAM role is a secure and convenient way to allow container instance configuration at launch time\. You can store a copy of your `ecs.config` file in a private bucket, use Amazon EC2 user data to install the AWS CLI and then copy your configuration information to `/etc/ecs/ecs.config` when the instance launches\.

For more information about creating an `ecs.config` file, storing it in Amazon S3, and launching instances with this configuration, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3)\.

**To allow Amazon S3 read\-only access for your container instance role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Choose the IAM role you use for your container instances \(this role is likely titled `ecsInstanceRole`\)\. For more information, see [Amazon ECS Container Instance IAM Role](#instance_IAM_role)\.

1. Choose the **Permissions** tab, then **Attach policy**\.

1. On the **Attach policy** page, type `S3` into the **Filter: Policy type** field to narrow the policy results\.

1. Check the box to the left of the **AmazonS3ReadOnlyAccess** policy and click **Attach policy**\.
**Note**  
This policy allows read\-only access to all Amazon S3 resources\. For more restrictive bucket policy examples, see [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html) in the Amazon Simple Storage Service Developer Guide\.