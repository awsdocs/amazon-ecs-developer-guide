# What is Amazon Elastic Container Service?<a name="Welcome"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast container management service that makes it easy to run, stop, and manage containers on a cluster\. Your containers are defined in a task definition that you use to run individual tasks or tasks within a service\. In this context, a service is a configuration that enables you to run and maintain a specified number of tasks simultaneously in a cluster\. You can run your tasks and services on a serverless infrastructure that is managed by AWS Fargate\. Alternatively, for more control over your infrastructure, you can run your tasks and services on a cluster of Amazon EC2 instances that you manage\.

Amazon ECS enables you to launch and stop your container\-based applications by using simple API calls\. You can also retrieve the state of your cluster from a centralized service and have access to many familiar Amazon EC2 features\.

You can schedule the placement of your containers across your cluster based on your resource needs, isolation policies, and availability requirements\. With Amazon ECS, you don't have to operate your own cluster management and configuration management systems or worry about scaling your management infrastructure\.

Amazon ECS can be used to create a consistent build and deployment experience, to manage and scale batch and Extract\-Transform\-Load \(ETL\) workloads, and to build sophisticated application architectures on a microservices model\. For more information about Amazon ECS use cases and scenarios, see [Container Use Cases](http://aws.amazon.com/containers/)\.

The AWS container services team maintains a public roadmap on GitHub\. The roadmap contains information about what the teams are working on and enables AWS customers to provide direct feedback\. For more information, see [AWS Containers Roadmap](https://github.com/aws/containers-roadmap)\.

## Features of Amazon ECS<a name="welcome-features"></a>

Amazon ECS is a regional service that simplifies running containers in a highly available manner across multiple Availability Zones within a Region\. You can create Amazon ECS clusters within a new or existing VPC\. After a cluster is up and running, you can create task definitions that define which container images run across your clusters\. Your task definitions are used to run tasks or create services\. Container images are stored in and pulled from container registries, for example, the [Amazon Elastic Container Registry](https://docs.aws.amazon.com/ecr)\.

The following diagram shows the architecture of an Amazon ECS environment run on AWS Fargate\.

![\[Diagram showing architecture of an Amazon ECS environment using the Fargate launch type.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

The following sections dive into these individual elements of the Amazon ECS architecture in more detail\.

### Containers and images<a name="welcome-containers-images"></a>

To deploy applications on Amazon ECS, your application components must be architected to run in *containers*\. A container is a standardized unit of software development that contains everything that your software application needs to run, including relevant code, runtime, system tools, and system libraries\. Containers are created from a read\-only template called an *image*\.

Images are typically built from a Dockerfile, which is a plaintext file that specifies all of the components that are included in the container\. After being built, these images are stored in a *registry* where they then can be downloaded and run on your cluster\. For more information about container technology, see [Docker basics for Amazon ECS](docker-basics.md)\.

![\[Diagram showing Docker image creation and registration within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containers.png)

### Task definitions<a name="welcome-task-definitions"></a>

To prepare your application to run on Amazon ECS, you must create a *task definition*\. The task definition is a text file \(in JSON format\) that describes one or more containers \(up to a maximum of ten\) that form your application\. The task definition can be thought of as a blueprint for your application\. It specifies various parameters for your application\. For example, these parameters can be used to indicate which containers should be used, which ports should be opened for your application, and what data volumes should be used with the containers in the task\. The specific parameters available for your task definition depend on the needs of your specific application\. For more information about creating task definitions, see [Amazon ECS task definitions](task_definitions.md)\.

The following is an example of a task definition that specifies the use of Fargate to launch a single container that runs an NGINX web server\. For a more extended example demonstrating the use of multiple containers in a task definition, see [Example task definitions](example_task_definitions.md)\. 

```
{
    "family": "webserver",
    "containerDefinitions": [
        {
            "name": "web",
            "image": "nginx",
            "memory": "100",
            "cpu": "99"
        },
    ],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "networkMode": "awsvpc",
    "memory": "512",
    "cpu": "256",
}
```

### Tasks and scheduling<a name="welcome-task-sched"></a>

A *task* is the instantiation of a task definition within a cluster\. After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks to run on your cluster\.

The Amazon ECS task scheduler is responsible for placing tasks within your cluster\. There are several different scheduling options available\. For example, you can define a *service* that runs and maintains a specified number of tasks simultaneously\. For more information about the different scheduling options available, see [Scheduling Amazon ECS tasks](scheduling_tasks.md)\.

![\[Diagram showing task scheduling and placement within an Amazon ECS environment using the Fargate launch type.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-service-fargate.png)

### Clusters<a name="welcome-clusters"></a>

An Amazon ECS *cluster* is a logical grouping of tasks or services\. You can register one or more Amazon EC2 instances \(also referred to as *container instances*\) with your cluster to run tasks on them\. Or, you can use the serverless infrastructure that Fargate provides to run tasks\. When your tasks are run on Fargate, your cluster resources are also managed by Fargate\.

When you first use Amazon ECS, a default cluster is created for you\. You can create additional clusters in an account to keep your resources separate\.

For more information about creating clusters, see [Amazon ECS clusters](clusters.md)\. For more information about launching container instances and registering them with your clusters, see [Amazon ECS container instances](ECS_instances.md)\.

### Container agent<a name="welcome-agent"></a>

The *container agent* runs on each container instance within an Amazon ECS cluster\. The agent sends information about the resource's current running tasks and resource utilization to Amazon ECS\. It starts and stops tasks whenever it receives a request from Amazon ECS\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.

![\[Diagram showing container agent tasks within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containeragent-fargate.png)

## Getting started with Amazon ECS<a name="welcome-getstarted"></a>

To learn about the developer tools available for using Amazon ECS, see [Amazon ECS developer tools overview](ecs-developer-tools.md)\.

If you are using Amazon ECS for the first time, the AWS Management Console for Amazon ECS provides a first\-run wizard that steps you through defining a task definition for a web server, configuring a service, and launching your first Fargate task\. We strongly recommend that you use the first\-run wizard if you have little or no prior experience using Amazon ECS\. For more information, see the [Getting started with Amazon ECS using Fargate](getting-started-fargate.md) tutorial\.

Alternatively, you can install the AWS Command Line Interface \(AWS CLI\) to use Amazon ECS\. For more information, see [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.

## Related services<a name="welcome-related"></a>

Amazon ECS can be used along with the following AWS services:

**AWS Identity and Access Management**  
IAM \(Identity and Access Management\) is an access management service that helps you securely control access to AWS resources\. You can use IAM to control who is authenticated \(signed in\) and authorized \(has permissions\) to view or perform specific actions on resources\. In Amazon ECS, you can use IAM to control access at the container instance level using IAM roles and at the task level using IAM task roles\. For more information, see [Identity and access management for Amazon Elastic Container Service](security-iam.md)\. 

**Amazon EC2 Auto Scaling**  
Auto Scaling is a service that enables you to automatically scale out or in your tasks based on user\-defined policies, health status checks, and schedules\. You can use Auto Scaling with a Fargate task within a service to scale in response to a number of metrics or with an EC2 task to scale the container instances within your cluster\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

**Elastic Load Balancing**  
The Elastic Load Balancing service automatically distributes incoming application traffic across the tasks in your Amazon ECS service\. It enables you to achieve greater levels of fault tolerance in your applications, seamlessly providing the required amount of load\-balancing capacity needed to distribute application traffic\. You can use Elastic Load Balancing to create an endpoint that balances traffic across services in a cluster\. For more information, see [Service load balancing](service-load-balancing.md)\.

**Amazon Elastic Container Registry**  
Amazon ECR is a managed AWS Docker registry service that is secure, scalable, and reliable\. Amazon ECR supports private Docker repositories with resource\-based permissions using IAM so that specific users or tasks can access repositories and images\. Developers can use the Docker CLI to push, pull, and manage images\. For more information, see the [Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)\.

**AWS CloudFormation**  
 AWS CloudFormation gives developers and systems administrators an easy way to create and manage a collection of related AWS resources\. More specifically, it makes resource provisioning and updating more orderly and predictable\. You can define clusters, task definitions, and services as entities in an AWS CloudFormation script\. For more information, see [AWS CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/)\.

## Pricing<a name="welcome-pricing"></a>

Amazon ECS pricing is dependent on whether you're using AWS Fargate or Amazon EC2 infrastructure to host your containerized workloads\. When using Amazon ECS on AWS Outposts, the pricing follows the same model as when you're using Amazon EC2\. For more information, see [Amazon ECS Pricing](https://aws.amazon.com/ecs/pricing)\.

Amazon ECS and Fargate also offer Savings Plans that provide significant savings based on your AWS usage\. For more information, see the [Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)\.

To view your bill, go to the **Billing and Cost Management Dashboard** in the [AWS Billing and Cost Management console](https://console.aws.amazon.com/billing/)\. Your bill contains links to usage reports that provide additional details about your bill\. To learn more about AWS account billing, see [AWS Account Billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

If you have questions concerning AWS billing, accounts, and events, [contact AWS Support](https://aws.amazon.com/contact-us/)\.

For an overview of Trusted Advisor, a service that helps you optimize the costs, security, and performance of your AWS environment, see [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)\.