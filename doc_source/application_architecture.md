# Application architecture<a name="application_architecture"></a>

You can follow one of the two following models to run your containers:
+ Fargate launch type \- This is a serverless pay\-as\-you\-go option\. You can run containers without having to manage your infrastructure\.
+ EC2 launch type \- Configure and deploy EC2 instances in your cluster to run your containers\.

How you architect your application on Amazon ECS depends on several factors, with the launch type that you're using being a key differentiator\. We give the following guidance, broken down by launch type, which should assist in the process\.

## Using the Fargate launch type<a name="application_architecture_fargate"></a>

The Fargate launch type is suitable for the following workloads:
+ Large workloads that require low operational overhead 
+ Small workloads that have occasional burst
+ Tiny workloads
+ Batch workloads

When architecting your application to run on Amazon ECS using AWS Fargate, you must decide between deploying multiple containers into the same task definition and deploying containers separately in multiple task definitions\.

If the following conditions are required, we recommend deploying multiple containers into the same task definition:
+ Your containers share a common lifecycle \(that is, they're launched and terminated together\)\.
+ Your containers must run on the same underlying host \(that is, one container references the other on a localhost port\)\.
+ You require that your containers share resources\.
+ Your containers share data volumes\.

If these conditions aren't required, we recommend deploying containers separately in multiple task definitions\. This is because, by doing so, you can scale, provision, and deprovision them separately\.

## Using the EC2 launch type<a name="application_architecture_ec2"></a>

The EC2 launch type is suitable for large workloads that must be price optimized\.

When considering how to model task definitions and services using the EC2 launch type, we recommend that you consider what processes must run together and how you might go about scaling each component\.

As an example, suppose that an application consists of the following components:
+ A frontend service that displays information on a webpage
+ A backend service that provides APIs for the frontend service
+ A data store

For this example, create task definitions that group the containers that are used for a common purpose together\. Separate the different components into multiple and separate task definitions\. The following example cluster has three container instances that are running three front\-end service containers, two backend service containers, and one data store service container\. 

You can group related containers in a task definition, such as linked containers that must be run together\. For example, add a log streaming container to your front\-end service and include it in the same task definition\.

After you have your task definitions, you can create services from them to maintain the availability of your desired tasks\. For more information, see [Creating an Amazon ECS service](create-service.md)\. In your services, you can associate containers with Elastic Load Balancing load balancers\. For more information, see [Service load balancing](service-load-balancing.md)\. When your application requirements change, you can update your services to scale the number of desired tasks up or down\. Or, you can update your services to deploy newer versions of the containers in your tasks\. For more information, see [Updating a service](update-service.md)\.

![\[Application architecture example\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/application.png)