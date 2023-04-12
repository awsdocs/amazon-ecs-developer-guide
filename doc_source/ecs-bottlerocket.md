# Amazon ECS\-optimized Bottlerocket AMIs<a name="ecs-bottlerocket"></a>

The Amazon ECS\-optimized Bottlerocket AMI is built on top of [Bottlerocket](http://aws.amazon.com/bottlerocket/)\. Bottlerocket is a Linux based open\-source operating system that is purpose built by AWS for running containers on virtual machines or bare metal hosts\. The Amazon ECS\-optimized Bottlerocket AMI is secure and only includes the minimum number of packages that's required to run containers\. This improves resource usage, reduces security attack surface, and helps lower management overhead\. The Bottlerocket AMI is also integrated with Amazon ECS to help reduce the operational overhead involved in updating container instances in a cluster\. 

Bottlerocket differs from Amazon Linux in the following ways:
+ Bottlerocket doesn't include a package manager, and its software can only be run as containers\. Updates to Bottlerocket are both applied and can be rolled back in a single step, which reduces the likelihood of update errors\.
+ The primary mechanism to manage Bottlerocket hosts is with a container scheduler\. Unlike Amazon Linux, logging into individual Bottlerocket instances is intended to be an infrequent operation for advanced debugging and troubleshooting purposes only\.

For more information about Bottlerocket, see the [documentation](https://github.com/bottlerocket-os/bottlerocket/blob/develop/README.md) and [releases](https://github.com/bottlerocket-os/bottlerocket/releases) on GitHub\.

An Amazon ECS\-optimized AMI variant of the Bottlerocket operating system is provided as an AMI that you can use when launching Amazon ECS container instances\. 

## Amazon ECS\-optimized Bottlerocket AMI variants<a name="ecs-bottlerocket-variants"></a>

You can use the following Amazon ECS\-optimized Bottlerocket AMI variants for your Amazon EC2 instances:
+ `aws-ecs-1`
+ `aws-ecs-1-nvidia`

  For more information about the `aws-ecs-1-nvidia` variant, see [Announcing NVIDIA GPU support for Bottlerocket on Amazon ECS](https://aws.amazon.com/blogs/containers/announcing-nvidia-gpu-support-for-bottlerocket-on-amazon-ecs/)\.

## Considerations<a name="ecs-bottlerocket-considerations"></a>

Consider the following when using a Bottlerocket AMI with Amazon ECS\.
+ You can deploy to Amazon EC2 instances with x86 or Arm64 processors, but not to instances that have Inferentia chips\.
+ Bottlerocket images don't include an SSH server or a shell\. However, you can use out\-of\-band management tools to gain SSH administrator access and perform bootstrapping\. For more information, see these sections in the [bottlerocket README\.md](https://github.com/bottlerocket-os/bottlerocket) on GitHub:
  + [Exploration](https://github.com/bottlerocket-os/bottlerocket#exploration)
  + [Admin container](https://github.com/bottlerocket-os/bottlerocket#admin-container)
+ By default, Bottlerocket has a [control container](https://github.com/bottlerocket-os/bottlerocket-control-container) that's enabled\. This container runs the [AWS Systems Manager agent](https://github.com/aws/amazon-ssm-agent) that you can use to run commands or start shell sessions on Amazon EC2 Bottlerocket instances\. For more information, see [Setting up Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html) in the *AWS Systems Manager User Guide*\.
+ Bottlerocket is optimized for container workloads and has a focus on security\. Bottlerocket doesn't include a package manager and is immutable\. For information about the security features and guidance, see [Security Features](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_FEATURES.md) and [Security Guidance](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_GUIDANCE.md) on GitHub\.
+ The `awsvpc` network mode is supported for Bottlerocket AMI version `1.1.0` or later\.
+ The Bottlerocket AMIs don't support the `initProcessEnabled` task definition parameter\.
+ The Bottlerocket AMIs also don't support the following services and features:
  + App Mesh in task definitions
  + ECS Anywhere
  + ECS Exec
  + Amazon EFS file system volumes
  + Amazon EFS in encrypted mode and `awsvpc` network mode
  + Elastic Inference

## Retrieving an Amazon ECS\-optimized Bottlerocket AMI<a name="ecs-bottlerocket-retrieve-ami"></a>

You can retrieve the Amazon Machine Image \(AMI\) ID for Amazon ECS\-optimized AMIs by querying the AWS Systems Manager Parameter Store API\. Using this parameter, you don't need to manually look up Amazon ECS\-optimized AMI IDs\. For more information about the Systems Manager Parameter Store API, see [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)\. The user that you use must have the `ssm:GetParameter` IAM permission to retrieve the Amazon ECS\-optimized AMI metadata\.

### Retrieving the `aws-ecs-1` Bottlerocket AMI variant<a name="ecs-bottlerocket-aws-ecs-1-variant"></a>

You can retrieve the latest stable `aws-ecs-1` Bottlerocket AMI variant by AWS Region and architecture with the AWS CLI or the AWS Management Console\. 
+ **AWS CLI** – You can retrieve the image ID of the latest recommended Amazon ECS\-optimized Bottlerocket AMI with the following AWS CLI command by using the subparameter `image_id`\. Replace the `region` with the Region code that you want the AMI ID for\. For information about the supported AWS Regions, see [Finding an AMI](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md#finding-an-ami) on GitHub\. To retrieve a version other than the latest, replace `latest` with the version number\.
  + For the 64\-bit \(`x86_64`\) architecture:

    ```
    aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id" --query Parameter.Value --output text
    ```
  + For the 64\-bit Arm \(`arm64`\) architecture:

    ```
    aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id" --query Parameter.Value --output text
    ```
+ **AWS Management Console** – You can query for the recommended Amazon ECS\-optimized AMI ID using a URL in the AWS Management Console\. The URL opens the Amazon EC2 Systems Manager console with the value of the ID for the parameter\. In the following URL, replace `region` with the Region code that you want the AMI ID for\. For information about the supported AWS Regions, see [Finding an AMI](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md#finding-an-ami) on GitHub\.
  + For the 64\-bit \(`x86_64`\) architecture:

    ```
    https://console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/x86_64/latest/image_id/description?region=region#
    ```
  + For the 64\-bit Arm \(`arm64`\) architecture:

    ```
    https://console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1/arm64/latest/image_id/description?region=region#
    ```

### Retrieving the `aws-ecs-1-nvidia` Bottlerocket AMI variant<a name="ecs-bottlerocket-aws-ecs-1-nvidia-variants"></a>

You can retrieve the latest stable `aws-ecs-1-nvdia` Bottlerocket AMI variant by Region and architecture with the AWS CLI or the AWS Management Console\. 
+ **AWS CLI** – You can retrieve the image ID of the latest recommended Amazon ECS\-optimized Bottlerocket AMI with the following AWS CLI command by using the subparameter `image_id`\. Replace the `region` with the Region code that you want the AMI ID for\. For information about the supported AWS Regions, see [Finding an AMI](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md#finding-an-ami) on GitHub\. To retrieve a version other than the latest, replace `latest` with the version number\.
  + For the 64\-bit \(`x86_64`\) architecture:

    ```
    aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id" --query Parameter.Value --output text
    ```
  + For the 64 bit Arm \(`arm64`\) architecture:

    ```
    aws ssm get-parameter --region us-east-1 --name "/aws/service/bottlerocket/aws-ecs-1-nvidia/arm64/latest/image_id" --query Parameter.Value --output text
    ```
+ **AWS Management Console** – You can query for the recommended Amazon ECS optimized AMI ID using a URL in the AWS Management Console\. The URL opens the Amazon EC2 Systems Manager console with the value of the ID for the parameter\. In the following URL, replace `region` with the Region code that you want the AMI ID for\. For information about the supported AWS Regions, see [Finding an AMI](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md#finding-an-ami) on GitHub\.
  + For the 64 bit \(`x86_64`\) architecture:

    ```
    https://console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=region#
    ```
  + For the 64 bit Arm \(`arm64`\) architecture:

    ```
    https://console.aws.amazon.com/systems-manager/parameters/aws/service/bottlerocket/aws-ecs-1-nvidia/x86_64/latest/image_id/description?region=region#
    ```

## Launch a Bottlerocket container instance<a name="bottlerocket-launch"></a>

You can use the AWS CLI to launch the container instance\.

1. Create a file that's called `userdata.toml`\. This file is used for the instance user data\. Replace *cluster\-name* with the name of your cluster\.

   ```
   [settings.ecs]
   cluster = "cluster-name"
   ```

1. Use one of the commands that are included in [Retrieving an Amazon ECS\-optimized Bottlerocket AMI](#ecs-bottlerocket-retrieve-ami) to get the Bottlerocket AMI ID\. You use this in the following step\.

1. Run the following command to launch the Bottlerocket instance\. Remember to replace the following parameters:
   + Replace *subnet* with the ID of the private or public subnet that your instance will launch in\.
   + Replace *bottlerocket\_ami* with the AMI ID from the previous step\.
   + Replace *t3\.large* with the instance type that you want to use\.
   + Replace *region* with the Region code\.

   ```
   aws ec2 run-instances --key-name ecs-bottlerocket-example \
      --subnet-id subnet \
      --image-id bottlerocket_ami \
      --instance-type t3.large \
      --region region \
      --tag-specifications 'ResourceType=instance,Tags=[{Key=bottlerocket,Value=example}]' \
      --user-data file://userdata.toml \
      --iam-instance-profile Name=ecsInstanceRole
   ```

1. Run the following command to verify that the container instance is registered to the cluster\. When you run this command, remember to replace the following parameters:
   + Replace *cluster* with your cluster name\.
   + Replace *region* with your Region code\.

   ```
   aws ecs list-container-instances --cluster cluster-name --region region
   ```

For a detailed walkthrough of how to get started with the Bottlerocket operating system on Amazon ECS, see [Using a Bottlerocket AMI with Amazon ECS](https://github.com/bottlerocket-os/bottlerocket/blob/develop/QUICKSTART-ECS.md) on GitHub and Getting started with [Bottlerocket and Amazon ECS](https://aws.amazon.com/blogs/containers/getting-started-with-bottlerocket-and-amazon-ecs/) on the AWS blog site\.