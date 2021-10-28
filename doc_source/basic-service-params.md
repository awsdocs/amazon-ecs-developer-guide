# Step 1: Configuring basic service parameters<a name="basic-service-params"></a>

**Important**  
Amazon ECS has provided a new console experience for creating a service\. For more information, see [Creating a service using the new console](create-service-console-v2.md)\.

All services require some basic configuration parameters that define the service, such as the task definition to use, which cluster the service should run on, how many tasks should be placed for the service, and so on\. This is called the *service definition*\. For more information about the parameters defined in a service definition, see [Service definition parameters](service_definition_parameters.md)\.

This procedure covers creating a service with the basic service definition parameters that are required\. After you have configured these parameters, you can create your service or move on to the procedures for optional service definition configuration, such as configuring your service to use a load balancer\.

**Note**  
If your cluster is configured with a default capacity provider strategy, you will only be able to create a service using the default capacity provider strategy when using the console\. Likewise, if no default capacity provider is defined, you will only be able to use a launch type when creating a service using the console\. It is not currently possible to have a mixed strategy using both capacity providers and launch types in the console\.

**To configure the basic service definition parameters**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the Region that your cluster is in\.

1. In the navigation pane, choose **Task Definitions** and select the task definition from which to create your service\.

1. On the **Task Definition name** page, select the revision of the task definition from which to create your service\.

1. Review the task definition, and choose **Actions**, **Create Service**\.

1. On the **Configure service** page, complete the following steps\.

   1. Choose either a capacity provider strategy or a launch type\.
      + To use a **Capacity provider strategy**, choose **Switch to capacity provider strategy** and then choose whether your service should use the default capacity provider strategy defined for the cluster or a custom capacity provider strategy\. A capacity provider must already be associated with the cluster in order to be used in a custom capacity provider strategy\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.
      + To use a **Launch type**, choose **Switch to launch type** and select **FARGATE**, **EC2**, or **EXTERNAL**\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.

   1. For **Platform operating system**, if you chose the Fargate launch type, then select the platform operating system\. for example, **LINUX**\.

   1. For **Platform version**, if you chose a Fargate capacity provider or the Fargate launch type, then select the platform version to use\.
**Note**  
When the **LATEST** platform version is selected, we validate the operating system that was specified for the task, and then set the appropriate platform version\. If the Operating System is set to `Windows-Server-2019-Full` or `Windows-Server-2019-Core`, the `1.0.0` platform is used\. If theoperating system is Linux, the `1.4.0` platform version is used\.

   1. **Cluster**: Select the cluster in which to create your service\.

   1. **Service name**: Type a unique name for your service\.

   1. **Service type**: Select a scheduling strategy for your service\. For more information, see [Service scheduler concepts](ecs_services.md#service_scheduler)\.

   1. **Number of tasks**: If you chose the `REPLICA` service type, type the number of tasks to launch and maintain on your cluster\.
**Note**  
If your launch type is `EC2`, and your task definition uses static host port mappings on your container instances, then you need at least one container instance with the specified port available in your cluster for each task in your service\. This restriction does not apply if your task definition uses dynamic host port mappings with the `bridge` network mode\. For more information, see [portMappings](task_definition_parameters.md#ContainerDefinition-portMappings)\.

   1. If you are using the **Rolling update** deployment type, fill out the following deployment configuration parameters\. For more information on how these parameters are used, see [Deployment configuration](service_definition_parameters.md#sd-deploymentconfiguration)\.
      + **Minimum healthy percent**: Specify a lower limit on the number of your service's tasks that must remain in the `RUNNING` state during a deployment, as a percentage of the service's desired number of tasks \(rounded up to the nearest integer\)\.
      + **Maximum percent**: Specify an upper limit on the number of your service's tasks that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the service's desired number of tasks \(rounded down to the nearest integer\)\.

1. For **Deployment circuit breaker**, choose the deployment circuit breaker logic\. For more information, see [Using the deployment circuit breaker](deployment-type-ecs.md#deployment-circuit-breaker)\.

1. On the **Deployments** page, complete the following steps\.

   1. For **Deployment type**, choose whether your service should use a rolling update deployment or a blue/green deployment using AWS CodeDeploy\. For more information, see [Amazon ECS Deployment types](deployment-types.md)\.

   1. If you selected the blue/green deployment type, complete the following steps:

      1. For **Deployment configuration** choose the deployment configuration to use for the service\. This determines how traffic is shifted when your task set is updated\. For more information, see [Blue/Green deployment with CodeDeploy](deployment-type-bluegreen.md)

      1. For **Service role for CodeDeploy** choose the IAM service role for AWS CodeDeploy\. For more information, see [Amazon ECS CodeDeploy IAM Role](codedeploy_IAM_role.md)

1. \(Optional\) If you selected the EC2 launch type and the `REPLICA` service type, for **Task Placement**, you can specify how tasks are placed using task placement strategies and constraints\. Choose from the following options\. For more information, see [Amazon ECS task placement](task-placement.md)\.
   + **AZ Balanced Spread** \- Distribute tasks across Availability Zones and across container instances in the Availability Zone\.
   + **AZ Balanced BinPack** \- Distribute tasks across Availability Zones and across container instances with the least available memory\.
   + **BinPack** \- Distribute tasks based on the least available amount of CPU or memory\.
   + **One Task Per Host** \- Place, at most, one task from the service on each container instance\.
   + **Custom** \- Define your own task placement strategy\. See [Amazon ECS task placement](task-placement.md) for examples\.

1. In the **Task tagging configuration** section, complete the following steps:

   1. Select **Enable ECS managed tags** if you want Amazon ECS to automatically tag the tasks in the service with the Amazon ECS managed tags\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

   1. For **Propagate tags from**, select one of the following:
      + **Do not propagate** – This option will not propagate any tags to the tasks in the service\.
      + **Service** – This option will propagate the tags specified on your service to each of the tasks in the service\.
      + **Task Definitions** – This option will propagate the tags specified in the task definition of a task to the tasks in the service\.
**Note**  
If you specify a tag with the same `key` in the **Tags** section, it will override the tag propagated from either the service or the task definition\.

1. In the **Tags** section, specify the key and value for each tag to associate with the task\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. Choose **Next step** and navigate to [Step 2: Configure a network](service-configure-network.md)\.