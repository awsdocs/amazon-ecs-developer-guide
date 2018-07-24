# Service Definition Parameters<a name="service_definition_parameters"></a>

A service definition defines which task definition to use with your service, how many instantiations of that task to run, and which load balancers \(if any\) to associate with your tasks\.

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
    "launchType": "EC2",
    "platformVersion": "",
    "role": "",
    "deploymentConfiguration": {
        "maximumPercent": 0,
        "minimumHealthyPercent": 0
    },
    "placementConstraints": [
        {
            "type": "memberOf",
            "expression": ""
        }
    ],
    "placementStrategy": [
        {
            "type": "random",
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
    "schedulingStrategy": "REPLICA"
}
```

**Note**  
You can create the above service definition template with the following AWS CLI command\.  

```
aws ecs create-service --generate-cli-skeleton
```

You can specify the following parameters in a service definition\.

`cluster`  
The short name or full Amazon Resource Name \(ARN\) of the cluster on which to run your service\. If you do not specify a cluster, the `default` cluster is assumed\.

`serviceName`  
The name of your service\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. Service names must be unique within a cluster, but you can have similarly named services in multiple clusters within a region or across multiple regions\.

`taskDefinition`  
The `family` and `revision` \(`family:revision`\) or full ARN of the task definition to run in your service\. If a `revision` is not specified, the latest `ACTIVE` revision is used\.

`loadBalancers`  
A load balancer object representing the load balancer to use with your service\. Currently, you are limited to one load balancer or target group per service\. After you create a service, the load balancer name or target group ARN, container name, and container port specified in the service definition are immutable\.  
For Classic Load Balancers, this object must contain the load balancer name, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance is registered with the load balancer specified here\.  
For Application Load Balancers and Network Load Balancers, this object must contain the load balancer target group ARN, the container name \(as it appears in a container definition\), and the container port to access from the load balancer\. When a task from this service is placed on a container instance, the container instance and port combination is registered as a target in the target group specified here\.    
`targetGroupArn`  
The full Amazon Resource Name \(ARN\) of the Elastic Load Balancing target group associated with a service\.  
`loadBalancerName`  
The name of the load balancer\.  
`containerName`  
The name of the container \(as it appears in a container definition\) to associate with the load balancer\.  
`containerPort`  
The port on the container to associate with the load balancer\. This port must correspond to a `containerPort` in the service's task definition\. Your container instances must allow ingress traffic on the `hostPort` of the port mapping\.

`serviceRegistries`  
The details of the service discovery configuration for your service\. For more information, see [Service Discovery](service-discovery.md)\.    
`registryArn`  
The Amazon Resource Name \(ARN\) of the service registry\. The currently supported service registry is Amazon Route 53 Auto Naming\. For more information, see [Service](http://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_Service.html)\.   
`port`  
The port value used if your service discovery service specified an SRV record\. This field is required if both the `awsvpc` network mode and SRV records are used\.  
`containerName`  
The container name value, already specified in the task definition, to be used for your service discovery service\. If the task definition that your service task specifies uses the `bridge` or `host` network mode, you must specify a `containerName` and `containerPort` combination from the task definition\. If the task definition that your service task specifies uses the `code ` network mode and a type SRV DNS record is used, you must specify either a `containerName` and `containerPort` combination or a `port` value, but not both\.  
`containerPort`  
The port value, already specified in the task definition, to be used for your service discovery service\. If the task definition your service task specifies uses the `bridge` or `host` network mode, you must specify a `containerName` and `containerPort` combination from the task definition\. If the task definition your service task specifies uses the `awsvpc` network mode and a type SRV DNS record is used, you must specify either a `containerName` and `containerPort` combination or a `port` value, but not both\.

`desiredCount`  
The number of instantiations of the specified task definition to place and keep running on your cluster\.

`clientToken`  
Unique, case\-sensitive identifier you provide to ensure the idempotency of the request\. Up to 32 ASCII characters are allowed\.

`launchType`  
The launch type on which to run your service\. If one is not specified, `EC2` is used by default\. For more information, see [Amazon ECS Launch Types](launch_types.md)\. 

`platformVersion`  
The platform version on which to run your service\. If one is not specified, the latest version \(`LATEST`\) is used by default\.  
AWS Fargate platform versions are used to refer to a specific runtime environment for the Fargate task infrastructure\. When specifying the `LATEST` platform version when running a task or creating a service, you get the most current platform version available for your tasks\. When you scale up your service, those tasks receive the platform version that was specified on the service's current deployment\. For more information, see [AWS Fargate Platform Versions](platform_versions.md)\.  
Platform versions are not specified for tasks using the EC2 launch type\.

`role`  
The name or full Amazon Resource Name \(ARN\) of the IAM role that allows Amazon ECS to make calls to your load balancer on your behalf\. This parameter is required if you are using a load balancer with your service\. If you specify the `role` parameter, you must also specify a load balancer object with the `loadBalancers` parameter\.  
If your specified role has a path other than `/`, then you must either specify the full role ARN \(this is recommended\) or prefix the role name with the path\. For example, if a role with the name `bar` has a path of `/foo/` then you would specify `/foo/bar` as the role name\. For more information, see [Friendly Names and Paths](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-friendly-names) in the *IAM User Guide*\.

`deploymentConfiguration`  
Optional deployment parameters that control how many tasks run during the deployment and the ordering of stopping and starting tasks\.    
`maximumPercent`  <a name="maximumPercent"></a>
The `maximumPercent` parameter represents an upper limit on the number of your service's tasks that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the `desiredCount` \(rounded down to the nearest integer\)\. This parameter enables you to define the deployment batch size\. For example, if your replica service has a `desiredCount` of four tasks and a `maximumPercent` value of 200%, the scheduler may start four new tasks before stopping the four older tasks \(provided that the cluster resources required to do this are available\)\. The default value for a replica service for `maximumPercent` is 200%\.  
If you are using a daemon service type, the `maximumPercent` should remain at 100%, which is the default value\.  
The maximum number of tasks during a deployment is the `desiredCount` multiplied by the `maximumPercent`/100, rounded down to the nearest integer value\.  
`minimumHealthyPercent`  <a name="minimumHealthyPercent"></a>
The `minimumHealthyPercent` represents a lower limit on the number of your service's tasks that must remain in the `RUNNING` state during a deployment, as a percentage of the `desiredCount` \(rounded up to the nearest integer\)\. This parameter enables you to deploy without using additional cluster capacity\. For example, if your service has a `desiredCount` of four tasks and a `minimumHealthyPercent` of 50%, the scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\. Tasks for services that *do not* use a load balancer are considered healthy if they are in the `RUNNING` state\. Tasks for services that *do* use a load balancer are considered healthy if they are in the `RUNNING` state and the container instance on which the load balancer is hosted is reported as healthy\. The default value for a replica service for `minimumHealthyPercent` is 50% in the AWS Management Console and 100% for the AWS CLI, the AWS SDKs, and the APIs\. The default value for a daemon service for `minimumHealthyPercent` is 0% for the AWS CLI, the AWS SDKs, and the APIs and 50% for the AWS Management Console\.  
The minimum number of healthy tasks during a deployment is the `desiredCount` multiplied by the `minimumHealthyPercent`/100, rounded up to the nearest integer value\.

`placementConstraints`  
An array of placement constraint objects to use for tasks in your service\. You can specify a maximum of 10 constraints per task \(this limit includes constraints in the task definition and those specified at run time\)\. If you are using the Fargate launch type, task placement constraints are not supported\.    
`type`  
The type of constraint\. Use `distinctInstance` to ensure that each task in a particular group is running on a different container instance\. Use `memberOf` to restrict the selection to a group of valid candidates\. The value `distinctInstance` is not supported in task definitions\.  
`expression`  
A cluster query language expression to apply to the constraint\. Note you cannot specify an expression if the constraint type is `distinctInstance`\. For more information, see [Cluster Query Language](cluster-query-language.md)\.

`placementStrategy`  
The placement strategy objects to use for tasks in your service\. You can specify a maximum of four strategy rules per service\.    
`type`  
The type of placement strategy\. The `random` placement strategy randomly places tasks on available candidates\. The `spread` placement strategy spreads placement across available candidates evenly based on the `field` parameter\. The `binpack` strategy places tasks on available candidates that have the least available amount of the resource that is specified with the `field` parameter\. For example, if you binpack on memory, a task is placed on the instance with the least amount of remaining memory \(but still enough to run the task\)\.  
`field`  
The field to apply the placement strategy against\. For the `spread` placement strategy, valid values are `instanceId` \(or `host`, which has the same effect\), or any platform or custom attribute that is applied to a container instance, such as `attribute:ecs.availability-zone`\. For the `binpack` placement strategy, valid values are `cpu` and `memory`\. For the `random` placement strategy, this field is not used\.

`networkConfiguration`  
The network configuration for the service\. This parameter is required for task definitions that use the `awsvpc` network mode to receive their own Elastic Network Interface, and it is not supported for other network modes\. If using the Fargate launch type, the `awsvpc` network mode is required\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.    
`awsvpcConfiguration`  
An object representing the subnets and security groups for a task or service\.    
`subnets`  
The subnets associated with the task or service\.  
`securityGroups`  
The security groups associated with the task or service\. If you do not specify a security group, the default security group for the VPC is used\.  
`assignPublicIP`  
Whether the task's elastic network interface receives a public IP address\.

`healthCheckGracePeriodSeconds`  
The period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks after a task has first started\. This is only valid if your service is configured to use a load balancer\. If your service's tasks take a while to start and respond to health checks, you can specify a health check grace period of up to 7,200 seconds during which the ECS service scheduler ignores the health check status\. This grace period can prevent the ECS service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\.

`schedulingStrategy`  
The scheduling strategy to use\. For more information, see [Service Scheduler Concepts](ecs_services.md#service_scheduler)\.  
There are two service scheduler strategies available:  
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](ecs_services.md#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](ecs_services.md#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.