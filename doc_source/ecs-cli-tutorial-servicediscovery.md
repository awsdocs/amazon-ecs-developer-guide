# Tutorial: Creating an Amazon ECS Service That Uses Service Discovery Using the Amazon ECS CLI<a name="ecs-cli-tutorial-servicediscovery"></a>

This tutorial shows a simple walkthrough of creating an Amazon ECS service that is configured to use service discovery\. Many of the service discovery configuration values can be specified with either the ECS parameters file or flags\. When flags are used, they take precedence over the ECS parameters file if both are present\. When using the Amazon ECS CLI, the compose project name is used as the name for your ECS service\.

## Prerequisites<a name="ECS_CLI_tutorial_servicediscovery_prerequisites"></a>

It is expected that you have completed the following prerequisites before continuing on:
+ Set up an AWS account\.
+ Install the Amazon ECS CLI\. For more information, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Configure the Amazon ECS CLI<a name="ECS_CLI_tutorial_servicediscovery_configure"></a>

Before you can start this tutorial, you must install and configure the Amazon ECS CLI\. For more information, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

The Amazon ECS CLI requires credentials in order to make API requests on your behalf\. It can pull credentials from environment variables, an AWS profile, or an Amazon ECS profile\. For more information, see [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)\.

**To create an Amazon ECS CLI configuration**

1. Create a cluster configuration:

   ```
   ecs-cli configure --cluster ec2-tutorial --region us-east-1 --default-launch-type EC2 --config-name ec2-tutorial
   ```

1. Create a profile using your access key and secret key:

   ```
   ecs-cli configure profile --access-key AWS_ACCESS_KEY_ID --secret-key AWS_SECRET_ACCESS_KEY --profile-name ec2-tutorial
   ```
**Note**  
If this is the first time that you are configuring the Amazon ECS CLI, these configurations are marked as default\. If this is not your first time configuring the Amazon ECS CLI, see the [Amazon ECS Command Line Reference](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html) in the *Amazon Elastic Container Service Developer Guide* to set this as the default configuration and profile\.

## Create an Amazon ECS Service Configured to Use Service Discovery<a name="ECS_CLI_tutorial_servicediscovery_create"></a>

Use the following steps to create an Amazon ECS service that is configured to use service discovery with the Amazon ECS CLI\.

**To create an Amazon ECS service configured to use service discovery**

1. Create an Amazon ECS service named `backend` and create a private DNS namespace named `tutorial` within a VPC\. In this example, the task is using the `awsvpc` network mode, so the `container_name` and `container_port` values are not required\.

   ```
   ecs-cli compose --project-name backend service up --private-dns-namespace tutorial --vpc vpc-04deee8176dce7d7d --enable-service-discovery
   ```

   Output:

   ```
   INFO[0001] Using ECS task definition                     TaskDefinition="backend:1"
   INFO[0002] Waiting for the private DNS namespace to be created...
   INFO[0002] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   WARN[0033] Defaulting DNS Type to A because network mode was awsvpc
   INFO[0033] Waiting for the Service Discovery Service to be created...
   INFO[0034] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   INFO[0065] Created an ECS service                        service=backend taskDefinition="backend:1"
   INFO[0066] Updated ECS service successfully              desiredCount=1 serviceName=backend
   INFO[0081] (service backend) has started 1 tasks: (task 824b5a76-8f9c-4beb-a64b-6904e320630e).  timestamp="2018-09-12 00:00:26 +0000 UTC"
   INFO[0157] Service status                                desiredCount=1 runningCount=1 serviceName=backend
   INFO[0157] ECS Service has reached a stable state        desiredCount=1 runningCount=1 serviceName=backend
   ```

1. Create another service named `frontend` in the same private DNS namespace\. Because the namespace already exists, the Amazon ECS CLI uses it instead of creating a new one\.

   ```
   ecs-cli compose --project-name frontend service up --private-dns-namespace tutorial --vpc vpc-04deee8176dce7d7d --enable-service-discovery
   ```

   Output:

   ```
   INFO[0001] Using ECS task definition                     TaskDefinition="frontend:1"
   INFO[0002] Using existing namespace ns-kvhnzhb5vxplfmls
   WARN[0033] Defaulting DNS Type to A because network mode was awsvpc
   INFO[0033] Waiting for the Service Discovery Service to be created...
   INFO[0034] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   INFO[0065] Created an ECS service                        service=frontend taskDefinition="frontend:1"
   INFO[0066] Updated ECS service successfully              desiredCount=1 serviceName=frontend
   INFO[0081] (service frontend) has started 1 tasks: (task 824b5a76-8f9c-4beb-a64b-6904e320630e).  timestamp="2018-09-12 00:00:26 +0000 UTC"
   INFO[0157] Service status                                desiredCount=1 runningCount=1 serviceName=frontend
   INFO[0157] ECS Service has reached a stable state        desiredCount=1 runningCount=1 serviceName=frontend
   ```

1. Verify that the two services are able to discover each other within the VPC using DNS\. The DNS hostname uses the following format: `<service_discovery_service_name>.<service_discovery_namespace>`\. For this example, the `frontend` service can be discovered at `frontend.tutorial` and the `backend` service can be discovered at `backend.tutorial`\. Because these are private DNS namespaces, these DNS names only resolve when within the specified VPC\.

1. To update the service discovery settings, update the settings for the `frontend` service\. The values that can be updated are the DNS TTL and the value for the health check custom config failure threshold\.

   ```
   ecs-cli compose --project-name frontend service up --update-service-discovery --dns-type SRV --dns-ttl 120 --healthcheck-custom-config-failure-threshold 2
   ```

   Output:

   ```
   INFO[0001] Using ECS task definition                     TaskDefinition="frontend:1"
   INFO[0001] Updated ECS service successfully              desiredCount=1 serviceName=frontend
   INFO[0001] Service status                                desiredCount=1 runningCount=1 serviceName=frontend
   INFO[0001] ECS Service has reached a stable state        desiredCount=1 runningCount=1 serviceName=frontend
   INFO[0002] Waiting for your Service Discovery resources to be updated...
   INFO[0002] Cloudformation stack status                   stackStatus=UPDATE_IN_PROGRESS
   ```

1. To clean up, delete the Amazon ECS service and the service discovery resources\. When the `frontend` service is deleted, the Amazon ECS CLI automatically removes the associated service discovery service\.

   ```
   ecs-cli compose --project-name frontend service rm
   ```

   ```
   INFO[0000] Updated ECS service successfully              desiredCount=0 serviceName=frontend
   INFO[0001] Service status                                desiredCount=0 runningCount=1 serviceName=frontend
   INFO[0016] Service status                                desiredCount=0 runningCount=0 serviceName=frontend
   INFO[0016] (service frontend) has stopped 1 running tasks: (task 824b5a76-8f9c-4beb-a64b-6904e320630e).  timestamp="2018-09-12 00:37:25 +0000 UTC"
   INFO[0016] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=frontend
   INFO[0016] Deleted ECS service                           service=frontend
   INFO[0016] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=frontend
   INFO[0027] Waiting for your Service Discovery Service resource to be deleted...
   INFO[0027] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
   ```

1. To complete the cleanup, delete the `backend` service along with the private DNS namespace that was created with it\. The Amazon ECS CLI associates the AWS CloudFormation stack for the private DNS namespace with the Amazon ECS service for which it was created\. When the service is deleted, the namespace is also deleted\.

   ```
   ecs-cli compose --project-name backend service rm --delete-namespace
   ```