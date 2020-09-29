# Common Use Cases in Amazon ECS<a name="common_use_cases"></a>

This topic provides guidance for two common use cases in Amazon ECS: microservices and batch jobs\. Here you can find considerations and external resources that may be useful for getting your application running on Amazon ECS, and the common aspects of each solution\.

**Topics**
+ [Microservices](#microservices)
+ [Batch Jobs](#batch)

## Microservices<a name="microservices"></a>

Microservices are built with a software architectural method that decomposes complex applications into smaller, independent services\. Containers are optimal for running small, decoupled services, and they offer the following advantages:
+ Containers make services easy to model in an immutable image with all of your dependencies\.
+ Containers can use any application and any programming language\.
+ The container image is a versioned artifact, so you can track your container images to the source they came from\.
+ You can test your containers locally, and deploy the same artifact to scale\.

The following sections cover some of the aspects and challenges that you must consider when designing a microservices architecture to run on Amazon ECS\. You can also view the microservices reference architecture on GitHub\. For more information, see [Deploying Microservices with Amazon ECS, AWS CloudFormation, and an Application Load Balancer](https://github.com/awslabs/ecs-refarch-cloudformation)\.

**Topics**
+ [Auto Scaling](#microservices_autoscaling)
+ [Service Discovery](#microservices_service_discovery)
+ [Authorization and Secrets Management](#microservices_secrets)
+ [Logging](#microservices_logging)
+ [Continuous Integration and Continuous Deployment](#microservices_cicd)

### Auto Scaling<a name="microservices_autoscaling"></a>

The application load for your microservice architecture can change over time\. A responsive application can scale out or in, depending on actual or anticipated load\. Amazon ECS provides you with several tools to scale not only your services that are running in your clusters, but the actual clusters themselves\.

For example, Amazon ECS provides CloudWatch metrics for your clusters and services\. For more information, see [Amazon ECS CloudWatch metrics](cloudwatch-metrics.md)\. You can monitor the memory and CPU utilization for your clusters and services\. Then, use those metrics to trigger CloudWatch alarms that can automatically scale out your cluster when its resources are running low\. Scale them back in when you don't need as many resources\. For more information, see [Tutorial: Scaling container instances with CloudWatch alarms](cloudwatch_alarm_autoscaling.md)\.

In addition to scaling your cluster size, your Amazon ECS service can optionally be configured to use Service Auto Scaling to adjust its desired count up or down in response to CloudWatch alarms\. Service Auto Scaling is available in all regions that support Amazon ECS\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

### Service Discovery<a name="microservices_service_discovery"></a>

Service discovery is a key component of most distributed systems and service\-oriented architectures\. With service discovery, your microservice components are automatically discovered as they get created and terminated on a given infrastructure\. There are several approaches that you can use to make your services discoverable\. The following resources describe a few examples:
+ [Run Containerized Microservices with Amazon EC2 Container Service and Application Load Balancer](http://aws.amazon.com/blogs/compute/microservice-delivery-with-amazon-ecs-and-application-load-balancers/): This post describes how to use the dynamic port mapping and path\-based routing features of Elastic Load Balancing Application Load Balancers to provide service discovery for a microservice architecture\.
+ [Amazon Elastic Container Service \- Reference Architecture: Service Discovery](https://github.com/awslabs/ecs-refarch-service-discovery/): This Amazon ECS reference architecture provides service discovery to containers using CloudWatch Events, Lambda, and Route 53 private hosted zones\. 
+ [Service Discovery via Consul with Amazon ECS](http://aws.amazon.com/blogs/compute/service-discovery-via-consul-with-amazon-ecs/): This post shows how a third party tool called [Consul by HashiCorp](https://www.consul.io/) can augment the capabilities of Amazon ECS by providing service discovery for an ECS cluster \(complete with an example application\)\.

### Authorization and Secrets Management<a name="microservices_secrets"></a>

Managing secrets, such as database credentials for an application, has always been a challenging issue\. The [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/) post focuses on how to integrate the [IAM roles for tasks](task-iam-roles.md) functionality of Amazon ECS with the AWS Systems Manager Parameter Store\. Parameter Store provides a centralized store to manage your configuration data, whether it's plaintext data such as database strings or secrets such as passwords, encrypted through AWS Key Management Service\.

### Logging<a name="microservices_logging"></a>

You can configure your container instances to send log information to CloudWatch Logs\. This enables you to view different logs from your container instances in one convenient location\. For more information about getting started using CloudWatch Logs on your container instances that were launched with the Amazon ECS\-optimized AMI, see [Using CloudWatch Logs with container instances](using_cloudwatch_logs.md)\. 

You can configure the containers in your tasks to send log information to CloudWatch Logs\. This enables you to view different logs from your containers in one convenient location, and it prevents your container logs from taking up disk space on your container instances\. For more information about getting started using the `awslogs` log driver in your task definitions, see [Using the awslogs Log Driver](using_awslogs.md)\.

### Continuous Integration and Continuous Deployment<a name="microservices_cicd"></a>

Continuous integration and continuous deployment \(CICD\) is a common process for microservice architectures that are based on Docker containers\. You can create a pipeline that takes the following actions: 
+ Monitors changes to a source code repository
+ Builds a new Docker image from that source
+ Pushes the image to an image repository such as Amazon ECR or Docker Hub
+ Updates your Amazon ECS services to use the new image in your application

 The following resources outline how to do this in different ways:
+ [ECS Reference Architecture: Continuous Deployment](https://github.com/awslabs/ecs-refarch-continuous-deployment): This reference architecture demonstrates how to achieve continuous deployment of an application to Amazon ECS using CodePipeline, CodeBuild, and AWS CloudFormation\.
+ [Continuous Delivery Pipeline for Amazon ECS Using Jenkins, GitHub, and Amazon ECR](https://github.com/awslabs/aws-cicd-docker-containers): This AWS labs repository helps you set up and configure a continuous delivery pipeline for Amazon ECS using Jenkins, GitHub, and Amazon ECR\.
+ [Pipelines For Container Applications Made Easy with mu](http://aws.amazon.com/blogs/opensource/mu-pipelines-container-applications): This post on the AWS Open Source blog shows how to use [mu](https://github.com/stelligent/mu) to configure a continuous delivery pipeline for a container workload using Amazon ECS, CodePipeline, and CodeBuild\.

## Batch Jobs<a name="batch"></a>

Docker containers are particularly suited for batch job workloads\. Batch jobs are often short\-lived and embarrassingly parallel\. You can package your batch processing application into a Docker image so that you can deploy it anywhere, such as in an Amazon ECS task\. If you are interested in running batch job workloads, consider the following resources:
+ [AWS Batch:](https://aws.amazon.com/batch/) For fully managed batch processing at any scale, you should consider using AWS Batch\. AWS Batch enables developers, scientists, and engineers to easily and efficiently run hundreds of thousands of batch computing jobs on AWS\. AWS Batch dynamically provisions the optimal quantity and type of compute resources \(for example, CPU or memory\-optimized instances\) based on the volume and specific resource requirements of the batch jobs submitted\. For more information, see [the AWS Batch product detail pages](https://aws.amazon.com/batch/)\.
+ [Amazon ECS Reference Architecture: Batch Processing](https://github.com/awslabs/ecs-refarch-batch-processing): This reference architecture illustrates how to use AWS CloudFormation, Amazon S3, Amazon SQS, and CloudWatch alarms to handle batch processing on Amazon ECS\. 