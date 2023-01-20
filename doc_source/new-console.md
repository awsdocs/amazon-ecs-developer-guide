# New Amazon ECS console<a name="new-console"></a>

Welcome to the new and improved console experience—a quicker and easier way to deploy containerized applications, configure load balancing, networking, monitoring, and gives you the new workflows for the effective operations and troubleshooting\. 

While most of the classic console functionality is available in the new console, it is still a work in progress\. We also want to give customers time learn and migrate to the new console\. Therefore the classic console will remain available to opt\-in and you can toggle back to the classic console for the duration of your login session\.

## Configuration which requires the classic console<a name="classic-console-use-cases"></a>

We’re working continuously to improve the experience\. The sections below describe when you need to use the classic console\. There will be many more functionality added to the new console\.

### First run wizard<a name="classic-console-first-run"></a>

Although the first run wizard is not available, we have provided instructions on how to get started using the new console\. For more information, see [Getting started with Amazon ECS](getting-started.md)\.

### Clusters<a name="classic-console-clusters"></a>

The following cluster options require the classic console:
+ **Spot Instances** \- For the EC2 launch type, using Spot Instances for your cluster Auto Scaling group
+ **Creating a new VPC on cluster creation** \- You must use existing VPCs and subnets when you create a cluster
+ **Updating clusters**

### Task definitions<a name="classic-console-task-def"></a>

The following task definition options require the classic console:
+ **Docker labels**
+ **AWS App Mesh integration**
+ **FireLens customization**
+ **Elastic inference**
+ **Task placement constraints**
+ **Update a service with the new task definition revision**
+ **Private repository authentication** 
+ **Network modes** \- The following network modes are not supported:
  + **default**
  + **host**
  + **none**
+ **FSx for volumes**
+ **Container parameters** The following container parameters are not supported:
  + **Command and entry point overrides**
  + **Startup and dependency ordering**
  + **Individual container log configuration** Monitoring and logging has replaced this option
  + **Timeouts, ulimits, and network settings**

In addition only the CloudWatch log driver is supported\.

### Tasks<a name="classic-console-tasks"></a>

The following task options require the classic console:
+ **Scheduling tasks** \- You cannot create, view, list, or update scheduled tasks\.
+ **Run more like this**

### Services<a name="classic-console-services"></a>

The following service options require the classic console:
+ **Service Discovery** \- You can only view your Service Discovery configuration\. 
+ **Step scaling policies for service auto scaling**
+ **Update Service** \- You cannot update the `awsvpc` network configuration\.

### Container instances<a name="classic-console-instances"></a>

The following container instance options require the classic console:
+ **Instance attributes** \- You cannot view or edit custom metadata for your container instance

**Please send feedback**  
We’d appreciate your feedback on the new console\. We’ll use your feedback to continue improving the experience\. You can send us feedback directly from the Amazon ECS console, or use the **Provide feedback** link at the bottom of this page\.