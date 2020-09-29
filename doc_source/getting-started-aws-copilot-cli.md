# Getting started with AWS Copilot by deploying an Amazon ECS application<a name="getting-started-aws-copilot-cli"></a>


|  | 
| --- |
|  AWS Copilot is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues with AWS Copilot by connecting with us at [GitHub](https://github.com/aws/amazon-ecs-cli-v2) where you can open issues, provide feedback and report bugs\.  | 

Learn how to deploy an Amazon ECS application using AWS Copilot\.

## Prerequisites<a name="getting-started-aws-copilot-cli-prerequisites"></a>

Before you begin, complete the following prerequisites:
+ Set up an AWS account\. For more information see [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Install the AWS Copilot CLI\. Releases currently support Linux and macOS systems\. For more information, see [Installing the AWS Copilot CLI](AWS_Copilot.md#copilot-install)\.
+ Install and configure the AWS CLI\. For more information, see [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\.
+ Run `aws configure` to set up a default profile that the AWS Copilot CLI will use to manage your application and services\.
+ Install Docker\. For more information see [Get Started with Docker](https://www.docker.com/get-started)\.

## Deploy your application using one command<a name="getting-started-ecs-copilot-cli-deploy-one"></a>

Make sure that you have the AWS command line tool installed and have already run `aws configure` before you start\.

Deploy the application using the following command\.

```
git clone git@github.com:aws-samples/amazon-ecs-cli-sample-app.git demo-app && \ 
cd demo-app &&                               \
copilot init --app demo                      \
  --svc api                                  \
  --svc-type 'Load Balanced Web Service'     \
  --dockerfile './Dockerfile'                \
  --port 80                                  \
  --deploy
```

## Deploy your application step by step<a name="getting-started-ecs-copilot-cli-deploy"></a>

### Step 1: Configure your credentials<a name="getting-started-ecs-copilot-cli-config-cred"></a>

Run `aws configure` to set up a default profile that the AWS Copilot CLI will use to manage your application and services\.

```
aws configure
```

### Step 2: Clone the demo app<a name="getting-started-ecs-copilot-cli-get-app"></a>

Clone a simple Flask application and Dockerfile\.

```
git clone git@github.com:aws-samples/amazon-ecs-cli-sample-app.git demo-app
```

### Step 3: Set up your application<a name="getting-started-ecs-copilot-cli-setup-app"></a>

1. From within the demo\-app directory, run the `init` command\.

   ```
   copilot init
   ```

   AWS Copilot walks you through the setup of your **first application and service** with a series of terminal prompts, starting with **next step**\. If you have already used AWS Copilot to deploy applications, you are prompted to choose one from a list of application names\.

1. Name your application\.

   ```
   What would you like to name your application? [? for help]
   ```

   Enter *`demo`*\.

### Step 4: Set up your ECS application service<a name="getting-started-ecs-copilot-cli-setup-svc"></a>

1. You are prompted to choose a service type\. You are building a simple Flask application which serves a small API\.

   ```
   Which service type best represents your service's architecture? [Use arrows to move, type to filter, ? for more help]
                    > Load Balanced Web Service
                      Backend Service
   ```

   Choose *`Load Balanced Web Service`*\.

1. Provide a name for your service\.

   ```
   What do you want to name this Load Balanced Web Service? [? for help]
   ```

   Enter *`api`* for your service name\.

1. Select a Dockerfile\.

   ```
   Which Dockerfile would you like to use for api? [Use arrows to move, type to filter, ? for more help]
                    > ./Dockerfile
   ```

   Choose *`Dockerfile`*\.

1. Define port\.

   ```
   Which port do you want customer traffic sent to? [? for help] (80)
   ```

   Enter *`80`* or accept default\.

1. You will see a log showing the application resources being created\.

   ```
   Creating the infrastructure to manage services under application demo.
   ```

1. After the application resources are created, deploy a test environment\.

   ```
   Would you like to deploy a test environment? [? for help] (y/N)
   ```

   Enter *`y`*\.

   ```
   Proposing infrastructure changes for the test environment.
   ```

1. You will see a log displaying the status of your application deployment\.

   ```
   Note: It's best to run this command in the root of your Git repository.
   Welcome to the Copilot CLI! We're going to walk you through some questions
   to help you get set up with an application on ECS. An application is a collection of
   containerized services that operate together.
   
   Use existing application: No
   Application name: demo
   Service type: Load Balanced Web Service
   Service name: api
   Dockerfile: ./Dockerfile
   no EXPOSE statements in Dockerfile ./Dockerfile
   Port: 80
   Ok great, we'll set up a Load Balanced Web Service named api in application demo listening on port 80.
   
   ✔ Created the infrastructure to manage services under application demo.
   
   ✔ Wrote the manifest for service api at copilot/api/manifest.yml
   Your manifest contains configurations like your container size and port (:80).
   
   ✔ Created ECR repositories for service api.
   
   All right, you're all set for local development.
   Deploy: Yes
   
   ✔ Created the infrastructure for the test environment.
   - Virtual private cloud on 2 availability zones to hold your services     [Complete]
   - Virtual private cloud on 2 availability zones to hold your services     [Complete]
     - Internet gateway to connect the network to the internet               [Complete]
     - Public subnets for internet facing services                           [Complete]
     - Private subnets for services that can't be reached from the internet  [Complete]
     - Routing tables for services to talk with each other                   [Complete]
   - ECS Cluster to hold your services                                       [Complete]
   - Application load balancer to distribute traffic                         [Complete]
   ✔ Linked account aws_account_id and region region to application demo.
   
   ✔ Created environment test in region region under application demo.
   Sending build context to Docker daemon  74.24kB
   Step 1/2 : FROM nginx
    ---> 9beeba249f3e
   Step 2/2 : COPY index.html /usr/share/nginx/html
    ---> Using cache
    ---> c685b776e459
   Successfully built c685b776e459
   Successfully tagged aws_account_id.dkr.ecr.region.amazonaws.com/demo/api:cee7709
   WARNING! Your password will be stored unencrypted in /home/user/.docker/config.json.
   Configure a credential helper to remove this warning. See
   https://docs.docker.com/engine/reference/commandline/login/#credentials-store
   
   Login Succeeded
   The push refers to repository [aws_account_id.dkr.ecr.region.amazonaws.com/demo/api]
   592a5c0c47f1: Pushed
   6c7de695ede3: Pushed
   2f4accd375d9: Pushed
   ffc9b21953f4: Pushed
   cee7709: digest: sha_digest
   
   ✔ Deployed api, you can access it at http://demo-Publi-1OQ8VMS2VC2WG-561733989.region.elb.amazonaws.com/api.
   ```

### Step 5: Verify your application is running<a name="getting-started-ecs-copilot-cli-running"></a>

View the status of your application by using the following commands\.

List all of your AWS Copilot applications\.

```
copilot app ls
```

Show information about the environments and services in your application\.

```
copilot app show
```

Show information about your environments\.

```
copilot env ls
```

Show information about the service, including endpoints, capacity and related resources\.

```
copilot svc show
```

List of all the services in an application\.

```
copilot svc ls
```

Show logs of a deployed service\.

```
copilot svc logs
```

Show service status\.

```
copilot svc status
```

List available commands and options\.

```
copilot --help
```

```
copilot init --help
```

### Step 6: Clean up<a name="getting-started-ecs-copilot-cli-cleanup"></a>

Run the following command to delete and clean up all resources\.

```
copilot app delete --env-profiles test=default
```