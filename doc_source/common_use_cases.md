# Common use cases in Amazon ECS<a name="common_use_cases"></a>

The following are common use cases for Amazon ECS:
+ Microservices
+ Websites
+ Video rendering services
+ Machine learning

  You can use AWS Batch to farm out tasks across your containers\.

## Additional resources<a name="additional-resources"></a>

You can use Amazon ECS to create a consistent build and deployment experience, to manage and scale batch and Extract\-Transform\-Load \(ETL\) workloads, and to build sophisticated application architectures on a microservices model\. For more information about Amazon ECS use cases and scenarios, see [Container Use Cases](http://aws.amazon.com/containers/)\.

You can view the microservices reference architecture on GitHub\. For more information, see [Deploying Microservices with Amazon ECS, AWS CloudFormation, and an Application Load Balancer](https://github.com/awslabs/ecs-refarch-cloudformation)\.

The following resources outline how to implement continuous integration and deployment \(CI/CD\):
+ [ECS Reference Architecture: Continuous Deployment](https://github.com/awslabs/ecs-refarch-continuous-deployment): This reference architecture demonstrates how to achieve continuous deployment of an application to Amazon ECS using CodePipeline, CodeBuild, and AWS CloudFormation\.
+ [Continuous Delivery Pipeline for Amazon ECS Using Jenkins, GitHub, and Amazon ECR](https://github.com/awslabs/aws-cicd-docker-containers): This AWS labs repository helps you set up and configure a continuous delivery pipeline for Amazon ECS using Jenkins, GitHub, and Amazon ECR\.

The [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/) post focuses on how to integrate the [IAM roles for tasks](task-iam-roles.md) functionality of Amazon ECS with the AWS Systems Manager Parameter Store\. Parameter Store provides a centralized store to manage your configuration data, whether it's plaintext data such as database strings or secrets such as passwords, encrypted through AWS Key Management Service\.

The following resources outline how to make your services discoverable:
+ [Run Containerized Microservices with Amazon ECS Container Service and Application Load Balancer](http://aws.amazon.com/blogs/compute/microservice-delivery-with-amazon-ecs-and-application-load-balancers/): This post describes how to use the dynamic port mapping and path\-based routing features of Elastic Load Balancing Application Load Balancers\. This provides service discovery for a microservice architecture\.
+ [Amazon Elastic Container Service \- Reference Architecture: Service Discovery](https://github.com/awslabs/ecs-refarch-service-discovery/): This Amazon ECS reference architecture provides service discovery to containers using CloudWatch Events, Lambda, and Route 53 private hosted zones\. 
+ [Metrics and traces collection from Amazon ECS using AWS Distro for OpenTelemetry with dynamic service discovery](http://aws.amazon.com/blogs/containers/metrics-and-traces-collection-from-amazon-ecs-using-aws-distro-for-opentelemetry-with-dynamic-service-discovery/): This post demonstrates how to employ a single instance of an ADOT Collector to collect X\-Ray traces and Prometheus metrics from Amazon ECS services that were dynamically discovered using AWS Cloud Map\.