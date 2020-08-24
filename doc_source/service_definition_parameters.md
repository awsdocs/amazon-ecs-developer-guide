# Service definition parameters<a name="service_definition_parameters"></a>

A service definition defines how to run your Amazon ECS service\. The following parameters can be specified in a service definition\.

## Launch type<a name="sd-launchtype"></a>

`launchType`  
Type: String  
Valid values: `EC2` \| `FARGATE`  
Required: No  
The launch type on which to run your service\. If a launch type is not specified, `EC2` is used by default\. For more information, see [Amazon ECS launch types](launch_types.md)\.  
If a `launchType` is specified, the `capacityProviderStrategy` parameter must be omitted\.

## Capacity provider strategy<a name="sd-capacityproviderstrategy"></a>

`capacityProviderStrategy`  
Type: Array of objects  
Required: No  
The capacity provider strategy to use for the service\.  
A capacity provider strategy consists of one or more capacity providers along with the `base` and `weight` to assign to them\. A capacity provider must be associated with the cluster to be used in a capacity provider strategy\. The PutClusterCapacityProviders API is used to associate a capacity provider with a cluster\. Only capacity providers with an `ACTIVE` or `UPDATING` status can be used\.  
If a `capacityProviderStrategy` is specified, the `launchType` parameter must be omitted\. If no `capacityProviderStrategy` or `launchType` is specified, the `defaultCapacityProviderStrategy` for the cluster is used\.  
If specifying a capacity provider that uses an Auto Scaling group, the capacity provider must already be created\. New capacity providers can be created with the CreateCapacityProvider API operation\.  
To use a AWS Fargate capacity provider, specify either the `FARGATE` or `FARGATE_SPOT` capacity providers\. The AWS Fargate capacity providers are available to all accounts and only need to be associated with a cluster to be used\.  
The PutClusterCapacityProviders API operation is used to update the list of available capacity providers for a cluster after the cluster is created\.    
`capacityProvider`  <a name="capacityProvider"></a>
Type: String  
Required: Yes  
The short name or full ARN of the capacity provider\.  
`weight`  <a name="weight"></a>
Type: Integer  
Valid range: Integers between 0 and 1,000\.  
Required: No  
The weight value designates the relative percentage of the total number of tasks launched that should use the specified capacity provider\.  
For example, if you have a strategy that contains two capacity providers and both have a weight of 1, then when the base is satisfied, the tasks will be split evenly across the two capacity providers\. Using that same logic, if you specify a weight of 1 for *capacityProviderA* and a weight of 4 for *capacityProviderB*, then for every one task that is run using *capacityProviderA*, four tasks would use *capacityProviderB*\.  
`base`  <a name="base"></a>
Type: Integer  
Valid range: Integers between 0 and 100,000\.  
Required: No  
The base value designates how many tasks, at a minimum, to run on the specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\.

## Task definition<a name="sd-taskdefinition"></a>

`taskDefinition`  
Type: String  
Required: No  
The `family` and `revision` \(`family:revision`\) or full Amazon Resource Name \(ARN\) of the task definition to run in your service\. If a `revision` is not specified, the latest `ACTIVE` revision of the specified family is used\.  
A task definition must be specified when using the rolling update \(`ECS`\) deployment controller\.

## Platform version<a name="sd-platformversion"></a>

`platformVersion`  
Type: String  
Required: No  
The platform version on which your tasks in the service are running\. A platform version is only specified for tasks using the Fargate launch type\. If one is not specified, the latest version \(`LATEST`\) is used by default\.  
AWS Fargate platform versions are used to refer to a specific runtime environment for the Fargate task infrastructure\. When specifying the `LATEST` platform version when running a task or creating a service, you get the most current platform version available for your tasks\. When you scale up your service, those tasks receive the platform version that was specified on the service's current deployment\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.  
Platform versions are not specified for tasks using the EC2 launch type\.

## Cluster<a name="sd-cluster"></a>

`cluster`  
Type: String  
Required: No  
The short name or full Amazon Resource Name \(ARN\) of the cluster on which to run your service\. If you do not specify a cluster, the `default` cluster is assumed\.

## Service name<a name="sd-servicename"></a>

`serviceName`  
Type: String  
Required: Yes  
The name of your service\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. Service names must be unique within a cluster, but you can have similarly named services in multiple clusters within a Region or across multiple Regions\.

## Scheduling strategy<a name="sd-schedulingstrategy"></a>

`schedulingStrategy`  
Type: String  
Valid values: `REPLICA` \| `DAEMON`  
Required: No  
The scheduling strategy to use\. If no scheduling strategy is specified, the `REPLICA` strategy is used\. For more information, see [Service scheduler concepts](ecs_services.md#service_scheduler)\.  
There are two service scheduler strategies available:  
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](ecs_services.md#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. The service scheduler evaluates the task placement constraints for running tasks and will stop tasks that do not meet the placement constraints\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](ecs_services.md#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.

## Desired count<a name="sd-desiredcount"></a>

`desiredCount`  
Type: Integer  
Required: No  
The number of instantiations of the specified task definition to place and keep running on your cluster\.  
This parameter is required if the `REPLICA` scheduling strategy is used\. If the service uses the `DAEMON` scheduling strategy, this parameter is optional\.

## Deployment configuration<a name="sd-deploymentconfiguration"></a>

`deploymentConfiguration`  
Type: Object  
Required: No  
Optional deployment parameters that control how many tasks run during the deployment and the ordering of stopping and starting tasks\.    
`maximumPercent`  <a name="maximumPercent"></a>
Type: Integer  
Required: No  
If a service is using the rolling update \(`ECS`\) deployment type, the `maximumPercent` parameter represents an upper limit on the number of your service's tasks that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the `desiredCount` \(rounded down to the nearest integer\)\. This parameter enables you to define the deployment batch size\. For example, if your service is using the `REPLICA` service scheduler and has a `desiredCount` of four tasks and a `maximumPercent` value of 200%, the scheduler may start four new tasks before stopping the four older tasks \(provided that the cluster resources required to do this are available\)\. The default `maximumPercent` value for a service using the `REPLICA` service scheduler is 200%\.  
If your service is using the `DAEMON` service scheduler type, the `maximumPercent` should remain at 100%, which is the default value\.  
The maximum number of tasks during a deployment is the `desiredCount` multiplied by the `maximumPercent`/100, rounded down to the nearest integer value\.  
If a service is using either the blue/green \(`CODE_DEPLOY`\) or `EXTERNAL` deployment types and tasks that use the EC2 launch type, the **maximum percent** value is set to the default value and is used to define the upper limit on the number of the tasks in the service that remain in the `RUNNING` state while the container instances are in the `DRAINING` state\. If the tasks in the service use the Fargate launch type, the maximum percent value is not used, although it is returned when describing your service\.  
`minimumHealthyPercent`  <a name="minimumHealthyPercent"></a>
Type: Integer  
Required: No  
If a service is using the rolling update \(`ECS`\) deployment type, the `minimumHealthyPercent` represents a lower limit on the number of your service's tasks that must remain in the `RUNNING` state during a deployment, as a percentage of the `desiredCount` \(rounded up to the nearest integer\)\. This parameter enables you to deploy without using additional cluster capacity\. For example, if your service has a `desiredCount` of four tasks and a `minimumHealthyPercent` of 50%, the service scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\.   
For services that *do not* use a load balancer, the following should be noted:  
+ A service is considered healthy if all essential containers within the tasks in the service pass their health checks\.
+ If a task has no essential containers with a health check defined, the service scheduler will wait for 40 seconds after a task reaches a `RUNNING` state before the task is counted towards the minimum healthy percent total\.
+ If a task has one or more essential containers with a health check defined, the service scheduler will wait for the task to reach a healthy status before counting it towards the minimum healthy percent total\. A task is considered healthy when all essential containers within the task have passed their health checks\. The amount of time the service scheduler can wait for is determined by the container health check settings\. For more information, see [Health Check](task_definition_parameters.md#container_definition_healthcheck)\. 
For services are that *do* use a load balancer, the following should be noted:  
+ If a task has no essential containers with a health check defined, the service scheduler will wait for the load balancer target group health check to return a healthy status before counting the task towards the minimum healthy percent total\.
+ If a task has an essential container with a health check defined, the service scheduler will wait for both the task to reach a healthy status and the load balancer target group health check to return a healthy status before counting the task towards the minimum healthy percent total\.
The default value for a replica service for `minimumHealthyPercent` is 100%\. The default `minimumHealthyPercent` value for a service using the `DAEMON` service schedule is 0% for the AWS CLI, the AWS SDKs, and the APIs and 50% for the AWS Management Console\.  
The minimum number of healthy tasks during a deployment is the `desiredCount` multiplied by the `minimumHealthyPercent`/100, rounded up to the nearest integer value\.  
If a service is using either the blue/green \(`CODE_DEPLOY`\) or `EXTERNAL` deployment types and tasks that use the EC2 launch type, the **minimum healthy percent** value is set to the default value and is used to define the lower limit on the number of the tasks in the service that remain in the `RUNNING` state while the container instances are in the `DRAINING` state\. If the tasks in the service use the Fargate launch type, the minimum healthy percent value is not used, although it is returned when describing your service\.

## Deployment controller<a name="sd-deploymentcontroller"></a>

`deploymentController`  
Type: Object  
Required: No  
The deployment controller to use for the service\. If no deploymenet controller is specified, the `ECS` controller is used\. For more information, see [Amazon ECS Deployment types](deployment-types.md)\.    
`type`  
Type: String  
Valid values: `ECS` \| `CODE_DEPLOY` \| `EXTERNAL`  
Required: yes  
The deployment controller type to use\. There are three deployment controller types available:    
`ECS`  
The rolling update \(`ECS`\) deployment type involves replacing the current running version of the container with the latest version\. The number of containers Amazon ECS adds or removes from the service during a rolling update is controlled by adjusting the minimum and maximum number of healthy tasks allowed during a service deployment, as specified in the [deploymentConfiguration](#deploymentConfiguration)\.  
`CODE_DEPLOY`  
The blue/green \(`CODE_DEPLOY`\) deployment type uses the blue/green deployment model powered by CodeDeploy, which allows you to verify a new deployment of a service before sending production traffic to it\.  
`EXTERNAL`  
The external deployment type enables you to use any third party deployment controller for full control over the deployment process for an Amazon ECS service\.

## Task placement<a name="sd-taskplacement"></a>

`placementConstraints`  
Type: Array of objects  
Required: No  
An array of placement constraint objects to use for tasks in your service\. You can specify a maximum of 10 constraints per task \(this limit includes constraints in the task definition and those specified at run time\)\. If you are using the Fargate launch type, task placement constraints are not supported\.    
`type`  
Type: String  
Required: No  
The type of constraint\. Use `distinctInstance` to ensure that each task in a particular group is running on a different container instance\. Use `memberOf` to restrict the selection to a group of valid candidates\. The value `distinctInstance` is not supported in task definitions\.  
`expression`  
Type: String  
Required: No  
A cluster query language expression to apply to the constraint\. Note you cannot specify an expression if the constraint type is `distinctInstance`\. For more information, see [Cluster query language](cluster-query-language.md)\.

`placementStrategy`  
Type: Array of objects  
Required: No  
The placement strategy objects to use for tasks in your service\. You can specify a maximum of four strategy rules per service\.    
`type`  
Type: String  
Valid values: `random` \| `spread` \| `binpack`  
Required: No  
The type of placement strategy\. The `random` placement strategy randomly places tasks on available candidates\. The `spread` placement strategy spreads placement across available candidates evenly based on the `field` parameter\. The `binpack` strategy places tasks on available candidates that have the least available amount of the resource that is specified with the `field` parameter\. For example, if you binpack on memory, a task is placed on the instance with the least amount of remaining memory \(but still enough to run the task\)\.  
`field`  
Type: String  
Required: No  
The field to apply the placement strategy against\. For the `spread` placement strategy, valid values are `instanceId` \(or `host`, which has the same effect\), or any platform or custom attribute that is applied to a container instance, such as `attribute:ecs.availability-zone`\. For the `binpack` placement strategy, valid values are `cpu` and `memory`\. For the `random` placement strategy, this field is not used\.

## Tags<a name="sd-tags"></a>

`tags`  
Type: Array of objects  
Required: No  
The metadata that you apply to the service to help you categorize and organize them\. Each tag consists of a key and an optional value, both of which you define\. When a service is deleted, the tags are deleted as well\. A maximum of 50 tags can be applied to the service\. For more information, see [Tagging your Amazon ECS resources](ecs-using-tags.md)\.    
`key`  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Required: No  
One part of a key\-value pair that make up a tag\. A key is a general label that acts like a category for more specific tag values\.  
`value`  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Required: No  
The optional part of a key\-value pair that make up a tag\. A value acts as a descriptor within a tag category \(key\)\.

`enableECSManagedTags`  
Type: Boolean  
Valid values: `true` \| `false`  
Required: No  
Specifies whether to enable Amazon ECS managed tags for the tasks in the service\. If no value is specified, the default value is `false`\. For more information, see [Tagging your resources for billing](ecs-using-tags.md#tag-resources-for-billing)\.

`propagateTags`  
Type: String  
Valid values: `TASK_DEFINITION` \| `SERVICE`  
Required: No  
Specifies whether to copy the tags from the task definition or the service to the tasks in the service\. If no value is specified, the tags are not copied\. Tags can only be copied to the tasks within the service during service creation\. To add tags to a task after service creation, use the `TagResource` API action\.

## Network configuration<a name="sd-networkconfiguration"></a>

`networkConfiguration`  
Type: Object  
Required: No  
The network configuration for the service\. This parameter is required for task definitions that use the `awsvpc` network mode to receive their own Elastic Network Interface, and it is not supported for other network modes\. If using the Fargate launch type, the `awsvpc` network mode is required\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.\.    
`awsvpcConfiguration`  
Type: Object  
Required: No  
An object representing the subnets and security groups for a task or service\.    
`subnets`  
Type: Array of strings  
Required: Yes  
The subnets associated with the task or service\. There is a limit of 16 subnets that can be specified per `awsvpcConfiguration`\.  
`securityGroups`  
Type: Array of strings  
Required: No  
The security groups associated with the task or service\. If you do not specify a security group, the default security group for the VPC is used\. There is a limit of 5 security groups that can be specified per `awsvpcConfiguration`\.  
`assignPublicIP`  
Type: String  
Valid values: `ENABLED` \| `DISABLED`  
Required: No  
Whether the task's elastic network interface receives a public IP address\. If no value is specified, the default value of `DISABLED` is used\.

`healthCheckGracePeriodSeconds`  
Type: Integer  
Required: No  
The period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks, container health checks, and Route 53 health checks after a task enters a `RUNNING` state\. This is only valid if your service is configured to use a load balancer\. If your service has a load balancer defined and you do not specify a health check grace period value, the default value of `0` is used\.  
If your service's tasks take a while to start and respond to health checks, you can specify a health check grace period of up to 2,147,483,647 seconds during which the ECS service scheduler ignores the health check status\. This grace period can prevent the ECS service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\.

`loadBalancers`  
Type: Array of objects  
Required: No  
A load balancer object representing the load balancers to use with your service\. For services that use an Application Load Balancer or Network Load Balancer, there is a limit of five target groups you can attach to a service\.  
After you create a service, the load balancer name or target group ARN, container name, and container port specified in the service definition are immutable\.  
For Classic Load Balancers, this object must contain the load balancer name, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance is registered with the load balancer specified here\.  
For Application Load Balancers and Network Load Balancers, this object must contain the load balancer target group ARN, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance and port combination is registered as a target in the target group specified here\.    
`targetGroupArn`  
Type: String  
Required: No  
The full ARN of the Elastic Load Balancing target group associated with a service\.  
A target group ARN is only specified when using an Application Load Balancer or Network Load Balancer\. If you are using a Classic Load Balancer the target group ARN should be omitted\.  
`loadBalancerName`  
Type: String  
Required: No  
The name of the load balancer to associate with the service\.  
A load balancer name is only specified when using a Classic Load Balancer\. If you are using an Application Load Balancer or a Network Load Balancer the load balancer name parameter should be omitted\.  
`containerName`  
Type: String  
Required: No  
The name of the container \(as it appears in a container definition\) to associate with the load balancer\.  
`containerPort`  
Type: Integer  
Required: No  
The port on the container to associate with the load balancer\. This port must correspond to a `containerPort` in the task definition used by tasks in the service\. For tasks that use the EC2 launch type, the container instance must allow ingress traffic on the `hostPort` of the port mapping\.

`role`  
Type: String  
Required: No  
The short name or full ARN of the IAM role that allows Amazon ECS to make calls to your load balancer on your behalf\. This parameter is only permitted if you are using a load balancer with your service and your task definition does not use the `awsvpc` network mode\. If you specify the `role` parameter, you must also specify a load balancer object with the `loadBalancers` parameter\.  
If your specified role has a path other than `/`, then you must either specify the full role ARN \(this is recommended\) or prefix the role name with the path\. For example, if a role with the name `bar` has a path of `/foo/` then you would specify `/foo/bar` as the role name\. For more information, see [Friendly Names and Paths](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-friendly-names) in the *IAM User Guide*\.  
If your account has already created the Amazon ECS service\-linked role, that role is used by default for your service unless you specify a role here\. The service\-linked role is required if your task definition uses the awsvpc network mode, in which case you should not specify a role here\. For more information, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\.

`serviceRegistries`  
Type: Array of objects  
Required: No  
The details of the service discovery configuration for your service\. For more information, see [Service Discovery](service-discovery.md)\.    
`registryArn`  
Type: String  
Required: No  
The ARN of the service registry\. The currently supported service registry is AWS Cloud Map\. For more information, see [Working with Services](https://docs.aws.amazon.com/cloud-map/latest/dg/working-with-services.html) in the *AWS Cloud Map Developer Guide*\.  
`port`  
Type: Integer  
Required: No  
The port value used if your service discovery service specified an SRV record\. This field is required if both the `awsvpc` network mode and SRV records are used\.  
`containerName`  
Type: String  
Required: No  
The container name value, already specified in the task definition, to be used for your service discovery service\. If the task definition that your service task specifies uses the `bridge` or `host` network mode, you must specify a `containerName` and `containerPort` combination from the task definition\. If the task definition that your service task specifies uses the `awsvpc` network mode and a type SRV DNS record is used, you must specify either a `containerName` and `containerPort` combination or a `port` value, but not both\.  
`containerPort`  
Type: Integer  
Required: No  
The port value, already specified in the task definition, to be used for your service discovery service\. If the task definition your service task specifies uses the `bridge` or `host` network mode, you must specify a `containerName` and `containerPort` combination from the task definition\. If the task definition your service task specifies uses the `awsvpc` network mode and a type SRV DNS record is used, you must specify either a `containerName` and `containerPort` combination or a `port` value, but not both\.

## Client token<a name="sd-clienttoken"></a>

`clientToken`  
Type: String  
Required: No  
Unique, case\-sensitive identifier you provide to ensure the idempotency of the request\. Up to 32 ASCII characters are allowed\.

## Service definition template<a name="sd-template"></a>

The following shows the JSON representation of an Amazon ECS service definition\.

```
{
    "cluster": "",
    "serviceName": "",
    "taskDefinition": "",
    "loadBalancers": [
        {
            "targetGroupArn": "",
            "loadBalancerName": "",
            "containerName": "",
            "containerPort": 0
        }
    ],
    "serviceRegistries": [
        {
            "registryArn": "",
            "port": 0,
            "containerName": "",
            "containerPort": 0
        }
    ],
    "desiredCount": 0,
    "clientToken": "",
    "launchType": "FARGATE",
    "capacityProviderStrategy": [
        {
            "capacityProvider": "",
            "weight": 0,
            "base": 0
        }
    ],
    "platformVersion": "",
    "role": "",
    "deploymentConfiguration": {
        "maximumPercent": 0,
        "minimumHealthyPercent": 0
    },
    "placementConstraints": [
        {
            "type": "distinctInstance",
            "expression": ""
        }
    ],
    "placementStrategy": [
        {
            "type": "spread",
            "field": ""
        }
    ],
    "networkConfiguration": {
        "awsvpcConfiguration": {
            "subnets": [
                ""
            ],
            "securityGroups": [
                ""
            ],
            "assignPublicIp": "ENABLED"
        }
    },
    "healthCheckGracePeriodSeconds": 0,
    "schedulingStrategy": "REPLICA",
    "deploymentController": {
        "type": "CODE_DEPLOY"
    },
    "tags": [
        {
            "key": "",
            "value": ""
        }
    ],
    "enableECSManagedTags": true,
    "propagateTags": "SERVICE"
}
```

You can create this service definition template using the following AWS CLI command\.

```
aws ecs create-service --generate-cli-skeleton
```