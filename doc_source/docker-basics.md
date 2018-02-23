# Docker Basics for Amazon ECS<a name="docker-basics"></a>

Docker is a technology that allows you to build, run, test, and deploy distributed applications that are based on Linux containers\. Amazon ECS uses Docker images in task definitions to launch containers on EC2 instances in your clusters\. For Amazon ECS product details, featured customer case studies, and FAQs, see the [Amazon Elastic Container Service product detail pages](http://aws.amazon.com/ecs)\.

The documentation in this guide assumes that readers possess a basic understanding of what Docker is and how it works\. For more information about Docker, see [What is Docker?](http://aws.amazon.com/docker/) and the [Docker User Guide](https://docs.docker.com/engine/userguide/)\. 


+ [Installing Docker](#install_docker)
+ [Create a Docker Image](#docker-basics-create-image)
+ [\(Optional\) Push your image to Amazon Elastic Container Registry](#use-ecr)
+ [Next Steps](#docker_next_steps)

## Installing Docker<a name="install_docker"></a>

**Note**  
If you already have Docker installed, skip to [Create a Docker Image](#docker-basics-create-image)\.

Docker is available on many different operating systems, including most modern Linux distributions, like Ubuntu, and even Mac OSX and Windows\. For more information about how to install Docker on your particular operating system, go to the [Docker installation guide](https://docs.docker.com/engine/installation/#installation)\.

You don't even need a local development system to use Docker\. If you are using Amazon EC2 already, you can launch an Amazon Linux instance and install Docker to get started\.

**To install Docker on an Amazon Linux instance**

1. Launch an instance with the Amazon Linux AMI\. For more information, see [Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your instance\. For more information, see [Connect to Your Linux Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Update the installed packages and package cache on your instance\.

   ```
   sudo yum update -y
   ```

1. Install the most recent Docker Community Edition package\.

   ```
   sudo yum install -y docker
   ```

1. Start the Docker service\.

   ```
   sudo service docker start
   ```

1. Add the `ec2-user` to the `docker` group so you can execute Docker commands without using `sudo`\.

   ```
   sudo usermod -a -G docker ec2-user
   ```

1. Log out and log back in again to pick up the new `docker` group permissions\. You can accomplish this by closing your current SSH terminal window and reconnecting to your instance in a new one\. Your new SSH session will have the appropriate `docker` group permissions\.

1. Verify that the `ec2-user` can run Docker commands without `sudo`\.

   ```
   docker info
   ```
**Note**  
In some cases, you may need to reboot your instance to provide permissions for the `ec2-user` to access the Docker daemon\. Try rebooting your instance if you see the following error:  

   ```
   Cannot connect to the Docker daemon. Is the docker daemon running on this host?
   ```

## Create a Docker Image<a name="docker-basics-create-image"></a>

Amazon ECS task definitions use Docker images to launch containers on the container instances in your clusters\. In this section, you create a Docker image of a simple web application, and test it on your local system or EC2 instance, and then push the image to a container registry \(such as Amazon ECR or Docker Hub\) so you can use it in an ECS task definition\.

**To create a Docker image of a simple web application**

1. Create a file called `Dockerfile`\. A Dockerfile is a manifest that describes the base image to use for your Docker image and what you want installed and running on it\. For more information about Dockerfiles, go to the [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)\.

   ```
   touch Dockerfile
   ```

1. Edit the `Dockerfile` you just created and add the following content\.

   ```
   FROM ubuntu:12.04
   
   # Install dependencies
   RUN apt-get update -y
   RUN apt-get install -y apache2
   
   # Install apache and write hello world message
   RUN echo "Hello World!" > /var/www/index.html
   
   # Configure apache
   RUN a2enmod rewrite
   RUN chown -R www-data:www-data /var/www
   ENV APACHE_RUN_USER www-data
   ENV APACHE_RUN_GROUP www-data
   ENV APACHE_LOG_DIR /var/log/apache2
   
   EXPOSE 80
   
   CMD ["/usr/sbin/apache2", "-D",  "FOREGROUND"]
   ```

   This Dockerfile uses the Ubuntu 12\.04 image\. The `RUN` instructions update the package caches, install some software packages for the web server, and then write the "Hello World\!" content to the web server's document root\. The `EXPOSE` instruction exposes port 80 on the container, and the `CMD` instruction starts the web server\.

1. <a name="sample-docker-build-step"></a>Build the Docker image from your Dockerfile\.
**Note**  
Some versions of Docker may require the full path to your Dockerfile in the following command, instead of the relative path shown below\.

   ```
   docker build -t hello-world .
   ```

1. Run docker images to verify that the image was created correctly\.

   ```
   docker images --filter reference=hello-world
   ```

   Output:

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   hello-world         latest              e9ffedc8c286        4 minutes ago       258MB
   ```

1. Run the newly built image\. The `-p 80:80` option maps the exposed port 80 on the container to port 80 on the host system\. For more information about docker run, go to the [Docker run reference](https://docs.docker.com/engine/reference/run/)\.

   ```
   docker run -p 80:80 hello-world
   ```
**Note**  
Output from the Apache web server is displayed in the terminal window\. You can ignore the "`Could not reliably determine the server's fully qualified domain name`" message\.

1. Open a browser and point to the server that is running Docker and hosting your container\.

   + If you are using an EC2 instance, this is the **Public DNS** value for the server, which is the same address you use to connect to the instance with SSH\. Make sure that the security group for your instance allows inbound traffic on port 80\.

   + If you are running Docker locally, point your browser to [http://localhost/](http://localhost/)\.

   + If you are using docker\-machine on a Windows or Mac computer, find the IP address of the VirtualBox VM that is hosting Docker with the docker\-machine ip command, substituting *machine\-name* with the name of the docker machine you are using\.

     ```
     docker-machine ip machine-name
     ```

   You should see a web page with your "Hello World\!" statement\.

1. Stop the Docker container by typing **Ctrl \+ c**\.

## \(Optional\) Push your image to Amazon Elastic Container Registry<a name="use-ecr"></a>

Amazon ECR is a managed AWS Docker registry service\. Customers can use the familiar Docker CLI to push, pull, and manage images\. For Amazon ECR product details, featured customer case studies, and FAQs, see the [Amazon Elastic Container Registry product detail pages](http://aws.amazon.com/ecr)\.

**Note**  
This section requires the AWS CLI\. If you do not have the AWS CLI installed on your system, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

**To tag your image and push it to Amazon ECR**

1. Create an Amazon ECR repository to store your `hello-world` image\. Note the `repositoryUri` in the output\.

   ```
   aws ecr create-repository --repository-name hello-world
   ```

   Output:

   ```
   {
       "repository": {
           "registryId": "aws_account_id",
           "repositoryName": "hello-world",
           "repositoryArn": "arn:aws:ecr:us-east-1:aws_account_id:repository/hello-world",
           "createdAt": 1505337806.0,
           "repositoryUri": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world"
       }
   }
   ```

1. Tag the `hello-world` image with the `repositoryUri` value from the previous step\.

   ```
   docker tag hello-world aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world
   ```

1. Run the aws ecr get\-login \-\-no\-include\-email command to get the docker login authentication command string for your registry\. 
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\. If you are using Docker version 17\.06 or later, include the `--no-include-email` option after `get-login`\. If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr get-login --no-include-email
   ```

1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible by other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

1. Push the image to Amazon ECR with the `repositoryUri` value from the earlier step\.

   ```
   docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world
   ```

## Next Steps<a name="docker_next_steps"></a>

After the image push is finished, you can use your image in your Amazon ECS task definitions, which you can use to run tasks with\.

**Note**  
This section requires the AWS CLI\. If you do not have the AWS CLI installed on your system, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

**To register a task definition with the `hello-world` image**

1. Create a file called `hello-world-task-def.json` with the following contents, substituting the `repositoryUri` from the previous section for the `image` field\.

   ```
   {
       "family": "hello-world",
       "containerDefinitions": [
           {
               "name": "hello-world",
               "image": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world",
               "cpu": 10,
               "memory": 500,
               "portMappings": [
                   {
                       "containerPort": 80,
                       "hostPort": 80
                   }
               ],
               "entryPoint": [
                   "/usr/sbin/apache2",
                   "-D",
                   "FOREGROUND"
               ],
               "essential": true
           }
       ]
   }
   ```

1. Register a task definition with the `hello-world-task-def.json` file\.

   ```
   aws ecs register-task-definition --cli-input-json file://hello-world-task-def.json
   ```

   The task definition is registered in the `hello-world` family as defined in the JSON file\.

**To run a task with the `hello-world` task definition**
**Important**  
Before you can run tasks in Amazon ECS, you need to launch container instances into a default cluster\. For more information about how to set up and launch container instances, see [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) and [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ Use the following AWS CLI command to run a task with the `hello-world` task definition\.

  ```
  aws ecs run-task --task-definition hello-world
  ```