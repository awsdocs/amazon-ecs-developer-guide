# Application architecture<a name="application_architecture"></a>

How you architect your application on Amazon ECS depends on several factors, with the launch type you are using being a key differentiator\. We give the following guidance, broken down by launch type, which should assist in the process\.

## Using the Fargate launch type<a name="application_architecture_fargate"></a>

When architecting your application to run on Amazon ECS using AWS Fargate, the main question is when should you put multiple containers into the same task definition versus deploying containers separately in multiple task definitions\.

When the following conditions are required, we recommend that you deploy your containers in a single task definition:
+ Your containers share a common lifecycle \(that is, they are launched and terminated together\)\.
+ Your containers must run on the same underlying host \(that is, one container references the other on a localhost port\)\.
+ You require that your containers share resources\.
+ Your containers share data volumes\.

Otherwise, you should define your containers in separate tasks definitions so that you can scale, provision, and deprovision them separately\.

## Using the EC2 launch type<a name="application_architecture_ec2"></a>

When you’re considering how to model task definitions and services using the EC2 launch type, it helps to think about what processes need to run together and how to scale each component\.

As an example, imagine an application that consists of the following components:
+ A frontend service that displays information on a webpage
+ A backend service that provides APIs for the frontend service
+ A data store

In your development environment, you probably run all three of these containers together on your Docker host\. You might be tempted to use the same approach for your production environment, but this approach has several drawbacks:
+ Changes to one component can impact all three of the components, which may be a larger scope for the change than anticipated\.
+ Each component is more difficult to scale because you have to scale every container proportionally\.
+ Task definitions can only have 10 container definitions, but your application stack might require more definitions, either now or in the future\.
+ Every container in a task definition must land on the same container instance, which can limit your instance choices to the largest sizes\.

Given these drawbacks, you should create task definitions that group the containers that are used for a common purpose, and separate the different components into multiple, separate task definitions\. In this preceding example, three task definitions each specify one container\. The following example cluster \(illustrated in the figure below\) has three container instances registered with three front\-end service containers, two backend service containers, and one data store service container\.

![\[Application architecture example\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/application.png)

You can group related containers in a task definition, such as linked containers that must be run together\. For example, you could add a log streaming container to your front\-end service and include it in the same task definition\.

After you have your task definitions, you can create services from them to maintain the availability of your desired tasks\. For more information, see [Creating a service](create-service.md)\. In your services, you can associate containers with Elastic Load Balancing load balancers\. For more information, see [Service load balancing](service-load-balancing.md)\. When your application requirements change, you can update your services to scale the number of desired tasks up or down, or to deploy newer versions of the containers in your tasks\. For more information, see [Updating a service](update-service.md)\.