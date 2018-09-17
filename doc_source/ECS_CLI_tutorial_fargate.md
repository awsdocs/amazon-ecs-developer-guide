# Tutorial: Creating a Cluster with a Fargate Task Using the ECS CLI<a name="ECS_CLI_tutorial_fargate"></a>

This tutorial shows you how to set up a cluster and deploy a task using the Fargate launch type\. 

## Prerequisites<a name="ECS_CLI_tutorial_fargate_prereqs"></a>

It is expected that you have completed the following prerequisites before continuing on:
+ Set up an AWS account
+ Installed the ECS CLI\. For more information, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.
+ Installed and configured the AWS CLI\. For more information, see [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)

## Step 1: Create the Task Execution IAM Role<a name="ECS_CLI_tutorial_fargate_iam_role"></a>

Amazon ECS needs permissions so that your Fargate task can store logs in CloudWatch\. This permission is covered by the task execution IAM role\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

**To create the task execution IAM role using the AWS CLI**

1. Create a file named `task-execution-assume-role.json` with the following contents:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ecs-tasks.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Create the task execution role:

   ```
   aws iam --region us-east-1 create-role --role-name ecsTaskExecutionRole --assume-role-policy-document file://task-execution-assume-role.json
   ```

1. Attach the task execution role policy:

   ```
   aws iam --region us-east-1 attach-role-policy --role-name ecsTaskExecutionRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy
   ```

## Step 2: Configure the ECS CLI<a name="ECS_CLI_tutorial_fargate_configure"></a>

The ECS CLI requires credentials in order to make API requests on your behalf\. It can pull credentials from environment variables, an AWS profile, or an Amazon ECS profile\. For more information, see [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)\.

**To create an ECS CLI configuration**

1. Create a cluster configuration, which defines the AWS region to use, resource creation prefixes, and the cluster name to use with the Amazon ECS CLI:

   ```
   ecs-cli configure --cluster tutorial --region us-east-1 --default-launch-type FARGATE --config-name tutorial
   ```

1. Create a CLI profile using your access key and secret key:

   ```
   ecs-cli configure profile --access-key AWS_ACCESS_KEY_ID --secret-key AWS_SECRET_ACCESS_KEY --profile-name tutorial
   ```
**Note**  
If this is the first time that you are configuring the ECS CLI, these configurations are marked as default\. If this is not your first time configuring the ECS CLI, see [ecs\-cli configure default](cmd-ecs-cli-configure-default.md) and [ecs\-cli configure profile default](cmd-ecs-cli-configure-profile-default.md) to set this as the default configuration and profile\.

## Step 3: Create a Cluster and Security Group<a name="ECS_CLI_tutorial_fargate_cluster"></a>

**To create an ECS cluster and security group**

1. Create an Amazon ECS cluster with the ecs\-cli up command\. Because you specified Fargate as your default launch type in the cluster configuration, this command creates an empty cluster and a VPC configured with two public subnets\.

   ```
   ecs-cli up
   ```
**Note**  
This command may take a few minutes to complete as your resources are created\. Take note of the VPC and subnet IDs that are created as they are used later\.

1. Using the AWS CLI, create a security group using the VPC ID from the previous output:

   ```
   aws ec2 create-security-group --group-name "my-sg" --description "My security group" --vpc-id "VPC_ID"
   ```

1. Using AWS CLI, add a security group rule to allow inbound access on port 80:

   ```
   aws ec2 authorize-security-group-ingress --group-id "security_group_id" --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```

## Step 4: Create a Compose File<a name="ECS_CLI_tutorial_fargate_compose_create"></a>

For this step, create a simple Docker compose file that creates a WordPress application\. At this time, the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. This tutorial uses Docker compose v3\.

Here is the compose file, which you can name `docker-compose.yml`\. The `wordpress` container exposes port 80 for inbound traffic to the web server\. It also configures container logs to go to the CloudWatch log group created earlier\. This is the recommended best practice for Fargate tasks\.

```
version: '3'
services:
  wordpress:
    image: wordpress
    ports:
      - "80:80"
    logging:
      driver: awslogs
      options: 
        awslogs-group: tutorial
        awslogs-region: us-east-1
        awslogs-stream-prefix: wordpress
```

In addition to the Docker compose information, there are some parameters specific to Amazon ECS that you must specify for the service\. Using the VPC, subnet, and security group IDs from the previous step, create a file named `ecs-params.yml` with the following content:

```
version: 1
task_definition:
  task_execution_role: ecsTaskExecutionRole
  ecs_network_mode: awsvpc
  task_size:
    mem_limit: 0.5GB
    cpu_limit: 256
run_params:
  network_configuration:
    awsvpc_configuration:
      subnets:
        - "subnet ID 1"
        - "subnet ID 2"
      security_groups:
        - "security group ID"
      assign_public_ip: ENABLED
```

## Step 5: Deploy the Compose File to a Cluster<a name="ECS_CLI_tutorial_fargate_compose_deploy"></a>

After you create the compose file, you can deploy it to your cluster with ecs\-cli compose service up\. By default, the command looks for files called `docker-compose.yml` and `ecs-params.yml` in the current directory; you can specify a different docker compose file with the `--file` option, and a different ECS Params file with the `--ecs-params` option\. By default, the resources created by this command have the current directory in their titles, but you can override that with the `--project-name` option\. The `--create-log-groups` option creates the CloudWatch log groups for the container logs\.

```
ecs-cli compose --project-name tutorial service up --create-log-groups --cluster-config tutorial
```

## Step 6: View the Running Containers on a Cluster<a name="ECS_CLI_tutorial_fargate_view"></a>

After you deploy the compose file, you can view the containers that are running in the service with ecs\-cli compose service ps\.

```
ecs-cli compose --project-name tutorial service ps --cluster-config tutorial
```

Output:

```
WARN[0000] Skipping unsupported YAML option...           option name=networks
WARN[0000] Skipping unsupported YAML option for service...  option name=networks service name=wordpress
Name                                            State    Ports TaskDefinition
a06a6642-12c5-4006-b1d1-033994580605/wordpress  RUNNING  54.146.193.73:80->80/tcp  tutorial:9
```

In the above example, you can see the `wordpress` container from your compose file, and also the IP address and port of the web server\. If you point your web browser at that address, you should see the WordPress installation wizard\. Also in the output is the `task-id` value for the container\. Copy the task ID as you use it in the next step\.

## Step 7: View the Container Logs<a name="ECS_CLI_tutorial_fargate_view_running"></a>

View the logs for the task:

```
ecs-cli logs --task-id a06a6642-12c5-4006-b1d1-033994580605 --follow --cluster-config tutorial
```

**Note**  
The `--follow` option tells the ECS CLI to continuously poll for logs\.

## Step 8: Scale the Tasks on the Cluster<a name="ECS_CLI_tutorial_fargate_scale"></a>

You can scale up your task count to increase the number of instances of your application with ecs\-cli compose service scale\. In this example, the running count of the application is increased to two\.

```
ecs-cli compose --project-name tutorial service scale 2 --cluster-config tutorial
```

Now you should see two more containers in your cluster:

```
ecs-cli compose --project-name tutorial service ps --cluster-config tutorial
```

Output:

```
WARN[0000] Skipping unsupported YAML option...           option name=networks
WARN[0000] Skipping unsupported YAML option for service...  option name=networks service name=wordpress
Name                                            State    Ports        TaskDefinition
880f09ed-613d-44bf-99bb-42ca44f82904/wordpress  RUNNING  34.224.60.24:80->80/tcp   tutorial:9
a06a6642-12c5-4006-b1d1-033994580/wordpress  RUNNING  54.146.193.73:80->80/tcp  tutorial:9
```

## Step 9: Clean Up<a name="ECS_CLI_tutorial_fargate_cleanup"></a>

When you are done with this tutorial, you should clean up your resources so they do not incur any more charges\. First, delete the service so that it stops the existing containers and does not try to run any more tasks\.

```
ecs-cli compose --project-name tutorial service down --cluster-config tutorial
```

Now, take down your cluster, which cleans up the resources that you created earlier with ecs\-cli up\.

```
ecs-cli down --force --cluster-config tutorial
```