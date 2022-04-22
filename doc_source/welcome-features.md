# Amazon ECS components<a name="welcome-features"></a>

## Clusters<a name="welcome-clusters"></a>

An Amazon ECS *cluster* is a logical grouping of tasks or services\. You can use clusters to isolate your applications\. This way, they don't use the same underlying infrastructure\. When your tasks are run on Fargate, your cluster resources are also managed by Fargate\. 

## Containers and images<a name="welcome-containers-images"></a>

To deploy applications on Amazon ECS, your application components must be configured to run in *containers*\. A container is a standardized unit of software development that holds everything that your software application requires to run\. This includes relevant code, runtime, system tools, and system libraries\. Containers are created from a read\-only template that's called an *image*\.

Images are typically built from a Dockerfile\. A Dockerfile is a plaintext file that specifies all of the components that are included in the container\. After they're built, these images are stored in a *registry* where they can be downloaded from\. Then, after you download them, you can use them to run on your cluster\. For more information about container technology, see [Creating a container image for use on Amazon ECS](create-container-image.md)\.

![\[Diagram showing Docker image creation and registration within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containers.png)

## Task definitions<a name="welcome-task-definitions"></a>

A *task definition* is a text file that describes one or more containers that form your application\. It's in JSON format\. You can use it to describe up to a maximum of ten containers\. The task definition functions as a blueprint for your application\. It specifies the various parameters for your application\. For example, you can use it to specify parameters for the operating system, which containers to use, which ports to open for your application, and what data volumes to use with the containers in the task\. The specific parameters available for your task definition depend on the needs of your specific application\. 

Your entire application stack doesn't need to be on a single task definition\. In fact, we recommend spanning your application across multiple task definitions\. You can do this by combining related containers into their own task definitions, each representing a single component\. 

## Tasks<a name="welcome-task-sched"></a>

A *task* is the instantiation of a task definition within a cluster\. After you create a task definition for your application within Amazon ECS, you can specify the number of tasks to run on your cluster\. You can run a standalone task, or you can run a task as part of a service\.

## Services<a name="welcome-service"></a>

You can use an Amazon ECS *service* to run and maintain your desired number of tasks simultaneously in an Amazon ECS cluster\. How it works is that, if any of your tasks fail or stop for any reason, the Amazon ECS service scheduler launches another instance based on your task definition\. It does this to replace it and thereby maintain your desired number of tasks in the service\.

## Container agent<a name="welcome-agent"></a>

The *container agent* runs on each container instance within an Amazon ECS cluster\. The agent sends information about the current running tasks and resource utilization of your containers to Amazon ECS\. It starts and stops tasks whenever it receives a request from Amazon ECS\. For more information, see [Amazon ECS container agent](ECS_agent.md)\.

![\[Diagram showing container agent tasks within an Amazon ECS environment.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containeragent-fargate.png)

## Fargate architecture overview<a name="welcome-architecture"></a>

Amazon ECS is a Regional service that simplifies the management involved with running containers in a highly available manner across multiple Availability Zones within an AWS Region\. You can create Amazon ECS clusters within a new or existing VPC\. After a cluster is up and running, you can create task definitions that define which container images run across your clusters\. Your task definitions are used to run tasks or create services\. Container images are stored in and pulled from container registries, such as the [Amazon Elastic Container Registry](https://docs.aws.amazon.com/ecr)\.

The following diagram shows the architecture of an Amazon ECS environment that runs on AWS Fargate\.

![\[Diagram showing architecture of an Amazon ECS environment using the Fargate launch type.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-fargate.png)