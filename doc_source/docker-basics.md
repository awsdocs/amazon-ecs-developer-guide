# Docker basics for Amazon ECS<a name="docker-basics"></a>

Docker is a technology that allows you to build, run, test, and deploy distributed applications that are based on Linux containers\. Amazon ECS uses Docker images in task definitions to launch containers on Amazon EC2 instances in your clusters\. For Amazon ECS product details, featured customer case studies, and FAQs, see the [Amazon Elastic Container Service product detail pages](http://aws.amazon.com/ecs)\.

The documentation in this guide assumes that readers possess a basic understanding of what Docker is and how it works\. For more information about Docker, see [What is Docker?](http://aws.amazon.com/docker/) and the [Docker overview](https://docs.docker.com/engine/docker-overview/)\.

## Installing Docker on Amazon Linux 2<a name="install_docker"></a>

**Note**  
If you already have Docker installed, skip to [Create a Docker Image](#docker-basics-create-image)\.

Docker is available on many different operating systems, including most modern Linux distributions, like Ubuntu, and even Mac OSX and Windows\. For more information about how to install Docker on your particular operating system, go to the [Docker installation guide](https://docs.docker.com/engine/installation/#installation)\.

You don't even need a local development system to use Docker\. If you are using Amazon EC2 already, you can launch an Amazon EC2 instance with an Amazon Linux 2 or Amazon Linux AMI and install Docker to get started\.

**To install Docker on an Amazon EC2 instance**

1. Launch an instance with the Amazon Linux 2 or Amazon Linux AMI\. For more information, see [Launching an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your instance\. For more information, see [Connect to Your Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Update the installed packages and package cache on your instance\.

   ```
   sudo yum update -y
   ```

1. Install the most recent Docker Community Edition package\.

   Amazon Linux 2

   ```
   sudo amazon-linux-extras install docker
   ```

   Amazon Linux\.

   ```
   sudo yum install docker
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

Amazon ECS task definitions use Docker images to launch containers on the container instances in your clusters\. In this section, you create a Docker image of a simple web application, and test it on your local system or Amazon EC2 instance, and then push the image to a container registry \(such as Amazon ECR or Docker Hub\) so you can use it in an Amazon ECS task definition\.

**To create a Docker image of a simple web application**

1. Create a file called `Dockerfile`\. A Dockerfile is a manifest that describes the base image to use for your Docker image and what you want installed and running on it\. For more information about Dockerfiles, go to the [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)\.

   ```
   touch Dockerfile
   ```

1. Edit the `Dockerfile` you just created and add the following content\.

   ```
   FROM ubuntu:18.04
   
   # Install dependencies
   RUN apt-get update && \
    apt-get -y install apache2
   
   # Install apache and write hello world message
   RUN echo 'Hello World!' > /var/www/html/index.html
   
   # Configure apache
   RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
    echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
    echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \ 
    echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \ 
    chmod 755 /root/run_apache.sh
   
   EXPOSE 80
   
   CMD /root/run_apache.sh
   ```

   This Dockerfile uses the Ubuntu 18\.04 image\. The `RUN` instructions update the package caches, install some software packages for the web server, and then write the "Hello World\!" content to the web server's document root\. The `EXPOSE` instruction exposes port 80 on the container, and the `CMD` instruction starts the web server\.

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
   hello-world         latest              e9ffedc8c286        4 minutes ago       241MB
   ```

1. Run the newly built image\. The `-p 80:80` option maps the exposed port 80 on the container to port 80 on the host system\. For more information about docker run, go to the [Docker run reference](https://docs.docker.com/engine/reference/run/)\.

   ```
   docker run -t -i -p 80:80 hello-world
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

## Push your image to Amazon Elastic Container Registry<a name="use-ecr"></a>

Amazon ECR is a managed AWS Docker registry service\. Customers can use the familiar Docker CLI to push, pull, and manage images\. For Amazon ECR product details, featured customer case studies, and FAQs, see the [Amazon Elastic Container Registry product detail pages](http://aws.amazon.com/ecr)\.

This section requires the following:
+ You have the AWS CLI installed and configured\. If you do not have the AWS CLI installed on your system, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.
+ Your user has the required IAM permissions to access the Amazon ECR service\. For more information, see [Amazon ECR Managed Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html)\.

**To tag your image and push it to Amazon ECR**

1. Create an Amazon ECR repository to store your `hello-world` image\. Note the `repositoryUri` in the output\.

   ```
   aws ecr create-repository --repository-name hello-repository --region region
   ```

   Output:

   ```
   {
       "repository": {
           "registryId": "aws_account_id",
           "repositoryName": "hello-repository",
           "repositoryArn": "arn:aws:ecr:region:aws_account_id:repository/hello-repository",
           "createdAt": 1505337806.0,
           "repositoryUri": "aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository"
       }
   }
   ```

1. Tag the `hello-world` image with the `repositoryUri` value from the previous step\.

   ```
   docker tag hello-world aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```

1. Run the aws ecr get\-login\-password command\. Specify the registry URI you want to authenticate to\. For more information, see [Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth) in the *Amazon Elastic Container Registry User Guide*\.

   ```
   aws ecr get-login-password | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```

   Output:

   ```
   Login Succeeded
   ```
**Important**  
If you receive an error, install or upgrade to the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

1. Push the image to Amazon ECR with the `repositoryUri` value from the earlier step\.

   ```
   docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```

## Clean up<a name="docker_cleanup"></a>

When you are done experimenting with your Amazon ECR image, you can delete the repository so you are not charged for image storage\.

```
aws ecr delete-repository --repository-name hello-repository --region region --force
```