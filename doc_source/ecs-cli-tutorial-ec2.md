# Tutorial: Creating a Cluster with an EC2 Task Using the Amazon ECS CLI<a name="ecs-cli-tutorial-ec2"></a>

This tutorial shows you how to set up a cluster and deploy a task using the EC2 launch type\.

## Prerequisites<a name="ECS_CLI_EC2_prerequisites"></a>

Complete the following prerequisites:
+ Complete the steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) and verify that your AWS user has either the permissions specified in the `AdministratorAccess` or the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ Install the Amazon ECS CLI\. For more information, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.
+ Install and configure the AWS CLI\. For more information, see [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)\.

## Step 1: Configure the Amazon ECS CLI<a name="ECS_CLI_tutorial_configure"></a>

Before you can start this tutorial, you must install and configure the Amazon ECS CLI\. For more information, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

The Amazon ECS CLI requires credentials in order to make API requests on your behalf\. It can pull credentials from environment variables, an AWS profile, or an Amazon ECS profile\. For more information, see [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)\.

**To create an Amazon ECS CLI configuration**

1. Create a cluster configuration:

   ```
   ecs-cli configure --cluster ec2-tutorial --default-launch-type EC2 --config-name ec2-tutorial --region us-west-2
   ```

1. Create a profile using your access key and secret key:

   ```
   ecs-cli configure profile --access-key AWS_ACCESS_KEY_ID --secret-key AWS_SECRET_ACCESS_KEY --profile-name ec2-tutorial-profile
   ```

## Step 2: Create Your Cluster<a name="ECS_CLI_tutorial_cluster_create"></a>

The first action you should take is to create a cluster of Amazon ECS container instances that you can launch your containers on with the ecs\-cli up command\. There are many options that you can choose to configure your cluster with this command, but most of them are optional\. In this example, you create a simple cluster of two `t2.medium` container instances that use the *id\_rsa* key pair for SSH access \(substitute your own key pair here\)\.

By default, the security group created for your container instances opens port 80 for inbound traffic\. You can use the `--port` option to specify a different port to open, or if you have more complicated security group requirements, you can specify an existing security group to use with the `--security-group` option\.

```
ecs-cli up --keypair id_rsa --capability-iam --size 2 --instance-type t2.medium --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

This command may take a few minutes to complete as your resources are created\. Now that you have a cluster, you can create a Docker compose file and deploy it\.

## Step 3: Create a Compose File<a name="ECS_CLI_tutorial_compose_create"></a>

For this step, create a simple Docker compose file that creates a simple PHP web application\. At this time, the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. This tutorial uses Docker Compose version 3\.

Here is the compose file, which you can call `docker-compose.yml`\. The `web` container exposes port 80 to the container instance for inbound traffic to the web server\. A logging configuration for the containers is also defined\.

```
version: '3'
services:
  web:
    image: amazon/amazon-ecs-sample
    ports:
      - "80:80"
    logging:
      driver: awslogs
      options: 
        awslogs-group: ec2-tutorial
        awslogs-region: us-west-2
        awslogs-stream-prefix: web
```

When using Docker Compose version 3 format, the CPU and memory specifications must be specified separately\. Create a file named `ecs-params.yml` with the following content:

```
version: 1
task_definition:
  services:
    web:
      cpu_shares: 100
      mem_limit: 524288000
```

## Step 4: Deploy the Compose File to a Cluster<a name="ECS_CLI_tutorial_compose_deploy"></a>

After you create the compose file, you can deploy it to your cluster with the ecs\-cli compose up command\. By default, the command looks for a compose file called `docker-compose.yml` and an optional ECS parameters file called `ecs-params.yml` in the current directory, but you can specify a different file with the `--file` option\. By default, the resources created by this command have the current directory in the title, but you can override that with the `--project-name project_name` option\. The `--create-log-groups` option creates the CloudWatch log groups for the container logs\.

```
ecs-cli compose up --create-log-groups --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

## Step 5: View the Running Containers on a Cluster<a name="ECS_CLI_tutorial_ps"></a>

After you deploy the compose file, you can view the containers that are running on your cluster with the ecs\-cli ps command\.

```
ecs-cli ps --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

Output:

```
Name                                               State    Ports                     TaskDefinition  Health
ec2-tutorial/53c943778bf048ce954a6cb96425adeb/web  RUNNING  54.201.208.32:80->80/tcp  ecscompose:1    UNKNOWN
```

In the above example, you can see the `web` container from your compose file, and also the IP address and port of the web server\. If you point a web browser to that address, you should see the PHP web application\.

## Step 6: Scale the Tasks on a Cluster<a name="ECS_CLI_tutorial_compose_scale"></a>

You can scale your task count up so you could have more instances of your application with the ecs\-cli compose scale command\. In this example, you can increase the count of your application to two\.

```
ecs-cli compose scale 2 --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

Now you should see two more containers in your cluster:

```
ecs-cli ps --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

Output:

```
Name                                               State    Ports                      TaskDefinition  Health
ec2-tutorial/53c943778bf048ce954a6cb96425adeb/web  RUNNING  54.201.208.32:80->80/tcp   ecscompose:1     UNKNOWN
ec2-tutorial/9451480d53534a129fe6794941ad63dc/web  RUNNING  52.43.118.109:80->80/tcp   ecscompose:1    UNKNOWN
```

## Step 7: Create an ECS Service from a Compose File<a name="ECS_CLI_tutorial_compose_service"></a>

Now that you know that your containers work properly, you can make sure that they are replaced if they fail or stop\. You can do this by creating a service from your compose file with the ecs\-cli compose service up command\. This command creates a task definition from the latest compose file \(if it does not already exist\) and creates an ECS service with it, with a desired count of 1\.

Before starting your service, stop the containers from your compose file with the ecs\-cli compose down command so that you have an empty cluster to work with\.

```
ecs-cli compose down --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

Now you can create your service\.

```
ecs-cli compose service up --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

## Step 8: View your Web Application<a name="ECS_CLI_tutorial_view"></a>

Enter the IP address for the task in your web browser and you should see a webpage that displays the **Simple PHP App** web application\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/web-app-output.png)

## Step 9: Clean Up<a name="ECS_CLI_tutorial_cleaning_up"></a>

When you are done with this tutorial, you should clean up your resources so they do not incur any more charges\. First, delete the service so that it stops the existing containers and does not try to run any more tasks\.

```
ecs-cli compose service rm --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```

Now, take down your cluster, which cleans up the resources that you created earlier with ecs\-cli up\.

```
ecs-cli down --force --cluster-config ec2-tutorial --ecs-profile ec2-tutorial-profile
```