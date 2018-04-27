# ecs\-cli up<a name="cmd-ecs-cli-up"></a>

## Description<a name="cmd-ecs-cli-up-description"></a>

Creates the ECS cluster \(if it does not already exist\) and the AWS resources required to set up the cluster\.

This command creates a new AWS CloudFormation stack called `amazon-ecs-cli-setup-cluster_name`\. You can view the progress of the stack creation in the AWS Management Console\. 

**Important**  
Some features described may only be available with the latest version of the ECS CLI\. To obtain the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-up-syntax"></a>

**ecs\-cli up \[\-\-verbose\] \[\-\-capability\-iam \| `--instance-role instance-profile-name`\] \[\-\-keypair *keypair\_name*\] \[`--size n`\] \[`--azs availability_zone_1,availability_zone_2`\] \[\-\-security\-group *security\_group\_id*\[,*security\_group\_id*\[,\.\.\.\]\]\] \[`--cidr ip_range`\] \[`--port port_number`\] \[`--subnets subnet_1,subnet_2`\] \[`--vpc vpc_id`\] \[\-\-instance\-type *instance\_type*\] \[\-\-image\-id *ami\_id*\] \[\-\-launch\-type *launch\_type*\] \[\-\-no\-associate\-public\-ip\-address\] \[\-\-force\] \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-empty\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-up-options"></a>


| Name | Description | 
| --- | --- | 
|  `--verbose, --debug`  |  Provides more verbose output for debugging purposes\. Required: No  | 
|  `--capability-iam`  |  Acknowledges that this command may create IAM resources\. This option is only used if using tasks with the EC2 launch type\. This parameter is required if you do not specify and instance profile name with `--instance-role`\. You cannot specify both options\. Required: No  | 
|  `--keypair keypair_name`  |  Specifies the name of an existing Amazon EC2 key pair to enable SSH access to the EC2 instances in your cluster\. This option is only used if using tasks with the EC2 launch type\. For more information about creating a key pair, see [Setting Up with Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html) in the *Amazon EC2 User Guide for Linux Instances*\. Type: String Required: No  | 
|  `--size n`  |  Specifies the number of instances to launch and register to the cluster\. This option is only used if using tasks with the EC2 launch type\. Type: Integer Default: 1 Required: No  | 
|  `--azs availability_zone_1,availability_zone_2`  |  Specifies a comma\-separated list of two VPC Availability Zones in which to create subnets \(these zones must have the `available` status\)\. We recommend this option if you do not specify a VPC ID with the `--vpc` option\.  Leaving this option blank can result in a failure to launch container instances when the randomly chosen zone is unavailable\.  Type: String Required: No  | 
|  `--security-group security_group_id[,security_group_id[,...]]`  |  Specifies a comma\-separated list of existing security groups to associate with your container instances\. If you do not specify a security group here, then a new one is created\. For more information, see [Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances*\. Required: No  | 
|  `--cidr ip_range`  |  Specifies a CIDR/IP range for the security group to use for container instances in your cluster\. This parameter is ignored if an existing security group is specified with the `--security-group` option\.  Type: CIDR/IP range Default: `0.0.0.0/0` Required: No  | 
|  `--port port_number`  |  Specifies a port to open on the security group to use for container instances in your cluster\. This parameter is ignored if an existing security group is specified with the `--security-group` option\.  Type: Integer Default: `80` Required: No  | 
|  `--subnets subnet_1,subnet_2`  |  Specifies a comma\-separated list of existing VPC subnet IDs in which to launch your container instances\. Type: String Required: This option is required if you specify a VPC with the `--vpc` option\.  | 
|  `--vpc vpc_id`  |  Specifies the ID of an existing VPC in which to launch your container instances\. If you specify a VPC ID, you must specify a list of existing subnets in that VPC with the `--subnets` option\. If you do not specify a VPC ID, a new VPC is created with two subnets\. Type: String Required: No  | 
|  `--instance-type instance_type`  |  Specifies the EC2 instance type for your container instances\. This option is only used if using tasks with the EC2 launch type\. For more information on EC2 instance types, see [Amazon EC2 Instances](https://aws.amazon.com/ec2/instance-types/)\. Type: String Default: `t2.micro` Required: No  | 
|  `--image-id ami_id`  |  Specifies the Amazon EC2 AMI ID to use for your container instances\. This option is only used if using tasks with the EC2 launch type\.  If no AMI ID is specified, the Amazon ECS CLI automatically retrieves the latest stable Amazon ECS\-optimized AMI by querying the SSM Parameter Store API during the cluster resource creation process\. This requires the user account that you are using to have the required SSM permissions\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.  Type: String Default: The latest stable Amazon ECS–optimized AMI for the specified region\. Required: No  | 
|  `--no-associate-public-ip-address`  |  Do not assign public IP addresses to new instances in this VPC\. Unless this option is specified, new instances in this VPC receive an automatically assigned public IP address\. This option is only used if using tasks with the EC2 launch type\. Required: No  | 
|  `--force, -f`  |  Forces the recreation of any existing resources that match your current configuration\. This option is useful for cleaning up stale resources from previous failed attempts\. Required: No  | 
|  `--instance-role, -f instance-profile-name`  |  Specifies a custom IAM instance profile name for instances in your cluster\. This option is only used if using tasks with the EC2 launch type\. This parameter is required if you do not specify the `--capability-iam` option\. You cannot specify both options\. Required: No  | 
| \-\-launch\-type launch\_type | Specifies the launch type to use\. Available options are FARGATE or EC2\. For more information about launch types, see [Amazon ECS Launch Types](launch_types.md)\. This overrides the default launch type stored in your cluster configuration\. Type: StringRequired: No | 
|  `--cluster, -c cluster_name`  |  Specifies the ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--empty, -e`  |  Specifies that an ECS cluster is created with no resources\. If other flags are also specified that would create resources, they are ignored and a warning is displayed\. Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-up-examples"></a>

### Creating a Cluster to Use with Tasks That Will Use the EC2 Launch Type<a name="cmd-ecs-cli-up-example-1"></a>

This example brings up a cluster of four `c4.large` instances and configures them to use the EC2 key pair called `id_rsa`\.

```
ecs-cli up --keypair keypair_name --capability-iam --size 4 --instance-type c4.large --launch-type EC2
```

Output:

```
INFO[0001] Using recommended Amazon Linux AMI with ECS Agent 1.17.3 and Docker version 17.12.1-ce
INFO[0000] Created cluster                               cluster=ecs-cli-ec2-demo
INFO[0000] Waiting for your cluster resources to be created
INFO[0001] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0061] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0121] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0181] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
Cluster creation succeeded.
VPC created: vpc-abcd1234
Security Group created: sg-abcd1234
Subnets created: subnet-abcd1234
Subnets created: subnet-dcba4321
```

### Creating a Cluster to Use with Tasks That Will Use the Fargate Launch Type<a name="cmd-ecs-cli-up-example-2"></a>

This example brings up a cluster for your Fargate tasks and creates a new VPC with two subnets\.

```
ecs-cli up --launch-type FARGATE --capability-iam
```

Output:

```
INFO[0001] Created cluster                               cluster=ecs-cli-fargate-demo region=us-west-2
INFO[0003] Waiting for your cluster resources to be created... 
INFO[0003] Cloudformation stack status                   stackStatus="CREATE_IN_PROGRESS"
INFO[0066] Waiting for your cluster resources to be created... 
INFO[0066] Cloudformation stack status                   stackStatus="CREATE_IN_PROGRESS"
VPC created: vpc-abcd1234
Subnets created: subnet-abcd1234
Subnets created: subnet-dcba4321
Cluster creation succeeded.
```

### Creating an Empty Cluster<a name="cmd-ecs-cli-up-example-3"></a>

This example brings up an empty cluster named `ecs-cli-empty-demo` with no resources\.

```
ecs-cli up --empty --cluster ecs-cli-empty-demo
```

Output:

```
INFO[0000] Created cluster                               cluster=ecs-cli-empty-demo region=us-east-1
Cluster creation succeeded.
```