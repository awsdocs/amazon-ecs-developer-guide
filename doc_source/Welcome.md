# What is Amazon Elastic Container Service?<a name="Welcome"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage containers on a cluster\. Your containers are defined in a task definition which you use to run individual tasks or as a service\. You can run your tasks and services on a serverless infrastructure that is managed by AWS Fargate or, for more control over your infrastructure, you can run your tasks and services on a cluster of Amazon EC2 instances that you manage\.

Amazon ECS enables you to launch and stop your container\-based applications with simple API calls\. You can also retrieve the state of your cluster from a centralized service and, for existing users of Amazon EC2, access many familiar Amazon EC2 features\.

You can schedule the placement of your containers across your cluster based on your resource needs, isolation policies, and availability requirements\. With Amazon ECS, you do not have to operate your own cluster management and configuration management systems or worry about scaling your management infrastructure\.

Amazon ECS can be used to create a consistent deployment and build experience, manage, and scale batch and Extract\-Transform\-Load \(ETL\) workloads, and build sophisticated application architectures on a microservices model\. For more information about Amazon ECS use cases and scenarios, see [Container Use Cases](http://aws.amazon.com/containers/)\.

The AWS container services team maintains a public roadmap on GitHub\. The roadmap contains information about what the teams are working on and allows AWS customers to provide direct feedback\. For more information, see [AWS Containers Roadmap](https://github.com/aws/containers-roadmap)\.

## Features of Amazon ECS<a name="welcome-features"></a>

Amazon ECS is a regional service that simplifies running containers in a highly available manner across multiple Availability Zones within a Region\. You can create Amazon ECS clusters within a new or existing VPC\. After a cluster is up and running, you can create task definitions that define which container images to run across your clusters\. Your task definitions are used to run tasks or create services\. Container images are stored in and pulled from container registries, for example Amazon Elastic Container Registry\.

The following diagram shows the architecture of an Amazon ECS environment run on AWS Fargate\.

![\[Diagram showing architecture of an Amazon ECS environment using the Fargate launch type.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)

The following sections dive into these individual elements of the Amazon ECS architecture in more detail\.

### Containers and images<a name="welcome-containers-images"></a>

To deploy applications on Amazon ECS, your application components must be architected to run in *containers*\. A container is a standardized unit of software development, containing everything that your software application needs to run: code, runtime, system tools, system libraries, etc\. Containers are created from a read\-only template called an *image*\.

Images are typically built from a Dockerfile, a plaintext file that specifies all of the components that are included in the container\. These images are then built and stored in a *registry* from which they can be downloaded and run on your cluster\. For more information about container technology, see [Docker basics for Amazon ECS](docker-basics.md)\.

![\[Diagram showing Docker image creation and registration within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containers.png)

### Task definitions<a name="welcome-task-definitions"></a>

To prepare your application to run on Amazon ECS, you create a *task definition*\. The task definition is a text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application\. It can be thought of as a blueprint for your application\. Task definitions specify various parameters for your application\. Examples of task definition parameters include which containers to use, which ports should be opened for your application, and what data volumes should be used with the containers in the task\. The specific parameters available for the task definition depend on the needs of your application\. For more information about creating task definitions, see [Amazon ECS Task definitions](task_definitions.md)\.

The following is an example of a task definition containing a single container that runs an NGINX web server that will run on Fargate\. For a more extended example demonstrating the use of multiple containers in a task definition, see [Example task definitions](example_task_definitions.md)\. 

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

A *task* is the instantiation of a task definition within a cluster\. After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks that will run on your cluster\.

The Amazon ECS task scheduler is responsible for placing tasks within your cluster\. There are several different scheduling options available\. For example, you can define a *service* that runs and maintains a specified number of tasks simultaneously\. For more information about the different scheduling options available, see [Scheduling Amazon ECS tasks](scheduling_tasks.md)\.

![\[Diagram showing task scheduling and placement within an Amazon ECS environment using the Fargate launch type.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-service-fargate.png)

### Clusters<a name="welcome-clusters"></a>

An Amazon ECS *cluster* is a logical grouping of tasks or services\. You can register one or more Amazon EC2 instances, also referred to as *container instances* with your cluster to run tasks on, or you can use the serverless infrastructure that Fargate provides\. When your tasks are run on Fargate, your cluster resources are managed by Fargate\.

When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate\.

For more information about creating clusters, see [Amazon ECS clusters](clusters.md)\. For more information about launching container instances and registering them with your clusters, see [Amazon ECS container instances](ECS_instances.md)\.

### Container agent<a name="welcome-agent"></a>

The *container agent* runs on each container instance within an Amazon ECS cluster\. It sends information about the resource's current running tasks and resource utilization to Amazon ECS, and starts and stops tasks whenever it receives a request from Amazon ECS\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.

![\[Diagram showing container agent tasks within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containeragent-fargate.png)

## How to get started with Amazon ECS<a name="welcome-getstarted"></a>

If you are using Amazon ECS for the first time, the AWS Management Console for Amazon ECS provides a first\-run wizard that steps you through defining a task definition for a web server, configuring a service, and launching your first Fargate task\. The first\-run wizard is highly recommended for users who have no prior experience with Amazon ECS\. For more information, see the [Getting started with Amazon ECS using Fargate](getting-started-fargate.md) tutorial\. 

Alternatively, you can install the AWS Command Line Interface \(AWS CLI\) to use Amazon ECS\. For more information, see [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.

## Related services<a name="welcome-related"></a>

Amazon ECS can be used along with the following AWS services:

**AWS Identity and Access Management**  
IAM is a web service that helps you securely control access to AWS resources for your users\. Use IAM to control who can use your AWS resources \(authentication\) and what resources they can use in which ways \(authorization\)\. In Amazon ECS, IAM can be used to control access at the container instance level using IAM roles, and at the task level using IAM task roles\. For more information, see [Identity and access management for Amazon Elastic Container Service](security-iam.md)\.

**Amazon EC2 Auto Scaling**  
Auto Scaling is a web service that enables you to automatically scale out or in your tasks based on user\-defined policies, health status checks, and schedules\. You can use Auto Scaling with a Fargate task within a service to scale in response to a number of metrics or with an EC2 task to scale the container instances within your cluster\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

**Elastic Load Balancing**  
Elastic Load Balancing automatically distributes incoming application traffic across the tasks in your Amazon ECS service\. It enables you to achieve greater levels of fault tolerance in your applications, seamlessly providing the required amount of load\-balancing capacity needed to distribute application traffic\. You can use Elastic Load Balancing to create an endpoint that balances traffic across services in a cluster\. For more information, see [Service load balancing](service-load-balancing.md)\.

**Amazon Elastic Container Registry**  
Amazon ECR is a managed AWS Docker registry service that is secure, scalable, and reliable\. Amazon ECR supports private Docker repositories with resource\-based permissions using IAM so that specific users or tasks can access repositories and images\. Developers can use the Docker CLI to push, pull, and manage images\. For more information, see the [Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)\.

**AWS CloudFormation**  
AWS CloudFormation gives developers and systems administrators an easy way to create and manage a collection of related AWS resources, provisioning and updating them in an orderly and predictable fashion\. You can define clusters, task definitions, and services as entities in an AWS CloudFormation script\. For more information, see [AWS CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/)\.

## Accessing Amazon ECS<a name="welcome-accessing"></a>

You can work with Amazon ECS in the following ways:

**AWS Management Console**  
The AWS Management Console is a browser\-based interface to manage Amazon ECS resources\. The console provides a visual overview of the service\. You can go through tutorials and walkthroughs easily while exploring new Amazon ECS features without needing additional tools\. For a tutorial that guides you through the console, see [Getting started with Amazon ECS](getting-started.md)\.

**AWS CloudFormation or Terraform**  
AWS CloudFormation or Terraform for Amazon ECS are powerful ways to define your infrastructure\-as\-code\. They enable you to easily track which version of your template or AWS CloudFormation stack is running at any time, and roll back to a previous version\. You can perform infrastructure and application deployments in the same automated fashion, making AWS CloudFormation and Terraform popular formats to deploy workloads to Amazon ECS from continuous delivery pipelines\.  
For more information about AWS CloudFormation, see [Creating Amazon ECS resources with AWS CloudFormation](creating-resources-with-cloudformation.md)\.

**AWS Copilot CLI**  
The AWS Copilot CLI is a command line interface that allows users to deploy and operate applications packaged in containers and environments on Amazon ECS\. It is a comprehensive tool that allows a customer to deploy applications directly from their source code without needing to understand AWS and Amazon ECS primitives such as Application Load Balancers, public subnets, tasks, services, and clusters\. AWS Copilot creates AWS resources on your behalf from opinionated service patterns, for example, a load balanced web service or backend service providing an out\-of\-the\-box production environment for containerized applications\. You can deploy through an AWS CodePipeline pipeline across multiple environments, accounts, or Regions all manageable within the CLI itself\. AWS Copilot enables you to take on operator tasks as well, such as viewing logs and the health of your service\.  
AWS Copilot is a single tool that enables users to focus on applications first\. For more information, see [Using the AWS Copilot command line interface](AWS_Copilot.md)\.

**AWS CDK**  
The AWS Cloud Development Kit \(AWS CDK\) is an open source software development framework to model and provision your cloud application resources using familiar programming languages\. AWS CDK provisions your resources in a safe, repeatable manner through AWS CloudFormation\. Using the CDK, users can generate their environment with fewer lines of code and use the same language as their applications\. Amazon ECS provides a module on the CDK named `ecs-patterns` which creates common architectures\. An available pattern is `ApplicationLoadBalancedFargateService()` which creates a cluster, task definition, and additional resources to run a load balanced Amazon ECS service on AWS Fargate\. For more information, see [Creating an Amazon ECS on AWS Fargate service using the AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/ecs_example.html) in the *AWS Cloud Development Kit \(AWS CDK\) Developer Guide*\.

**AWS CLI**  
The AWS Command Line Interface \(AWS CLI\) is a unified tool to manage your AWS services\. With a single tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts\. The Amazon ECS commands in the AWS CLI are a reflection of the Amazon ECS API\. Using the AWS CLI is beneficial to users that prefer a command line tool and scripting but also know exactly what action to take on their Amazon ECS resources or for users who want to learn more about the Amazon ECS APIs\.  
AWS provides two sets of command line tools: the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/) \(AWS CLI\) and the [AWS Tools for Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) and the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\.

**Amazon ECS CLI**  
The Amazon ECS CLI enables users to run their applications on Amazon ECS and AWS Fargate using the Docker Compose file format\. You can quickly provision resources, push and pull images using Amazon ECR, and monitor running applications on Amazon ECS or AWS Fargate\. You can also test containers running locally along with containers in the cloud within the CLI\. For more information, see [Using the Amazon ECS Command Line Interface](ECS_CLI.md)\.

**Docker CLI plugin for Amazon ECS**  
AWS and Docker have collaborated to make a simplified developer experience that enables you to deploy and manage containers on Amazon ECS directly from Docker tools\. You can now build and test your containers locally using Docker Desktop and Docker Compose, and then deploy them to Amazon ECS on Fargate through the same CLI\.  
The Docker CLI plugin for Amazon ECS is currently in Beta\. For more information, see [Docker CLI plugin for Amazon ECS](https://github.com/docker/ecs-plugin) on GitHub\.

**AWS SDKs**  
We also provide SDKs that enable you to access Amazon ECS from a variety of programming languages\. The SDKs automatically take care of tasks such as:  
+ Cryptographically signing your service requests
+ Retrying requests
+ Handling error responses
For more information about available SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/)\.

## Pricing<a name="welcome-pricing"></a>

Amazon ECS pricing is dependent on whether you are using AWS Fargate or Amazon EC2 infrastructure to host your containerized workloads\. When using Amazon ECS on AWS Outposts, the pricing follows the same model as using Amazon EC2\. For more information, see [Amazon ECS Pricing](https://aws.amazon.com/ecs/pricing)\.

Amazon ECS and Fargate also offer Savings Plans which provides significant savings on your AWS usage\. For more information, see the [Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)\.

To see your bill, go to the **Billing and Cost Management Dashboard** in the [AWS Billing and Cost Management console](https://console.aws.amazon.com/billing/)\. Your bill contains links to usage reports that provide details about your bill\. To learn more about AWS account billing, see [AWS Account Billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

If you have questions concerning AWS billing, accounts, and events, [contact AWS Support](https://aws.amazon.com/contact-us/)\.

For an overview of Trusted Advisor, a service that helps you optimize the costs, security, and performance of your AWS environment, see [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)\.