# Using Amazon ECS Parameters<a name="cmd-ecs-cli-compose-ecsparams"></a>

When using the `ecs-cli compose` or `ecs-cli compose service` commands to manage your Amazon ECS tasks and services, there are certain fields in an Amazon ECS task definition that do not correspond to fields in a Docker compose file\. You can specify those values using an ECS parameters file with the `--ecs-params` flag\. By default, the command looks for an ECS parameters file in the current directory named `ecs-params.yml`\. However, you can also specify a different file name or path to an ECS parameters file with the `--ecs-params` option\.

Currently, the file supports the follow schema:

```
version: 1
task_definition:
  ecs_network_mode: string
  task_role_arn: string
  task_execution_role: string
  task_size:
    cpu_limit: string
    mem_limit: string
  services:
    <service_name>:
      essential: boolean
      repository_credentials:
        credentials_parameter: string
      cpu_shares: integer
      mem_limit: string
      mem_reservation: string
      healthcheck:
        test: ["CMD", "curl -f http://localhost"]
        interval: string
        timeout: string
        retries: integer
        start_period: string
  docker_volumes:
      name: string
      scope: string
      autoprovision: 
      driver: string
      driver_opts: boolean
         string: string
      labels:
         string: string
run_params:
  network_configuration:
    awsvpc_configuration:
      subnets: 
        - subnet_id1 
        - subnet_id2
      security_groups: 
        - secgroup_id1
        - secgroup_id2
      assign_public_ip: ENABLED
  task_placement:
    strategy:
        type: string
        field: string
    constraints:
        type: string
        expression: string
  service_discovery:
    container_name: string
    container_port: integer
    private_dns_namespace:
        vpc: string
        id: string
        name: string
        description: string
    public_dns_namespace:
        id: string
        name: string
    service_discovery_service:
        name: string
        description: string
        dns_config:
          type: string
          ttl: integer
        healthcheck_custom_config:
          failure_threshold: integer
```

The fields listed under `task_definition` correspond to fields to be included in your Amazon ECS task definition\.
+ `ecs_network_mode` – Corresponds to `networkMode` in an ECS task definition\. Supported values are `none`, `bridge`, `host`, or `awsvpc`\. The default value is `bridge`\. If you are using task networking, this field must be set to `awsvpc`\. For more information, see [Network Mode](task_definition_parameters.md#network_mode)\.
+ `task_role_arn` – The name or full ARN of an IAM role to be associated with the task\. For more information, see [Task Role](task_definition_parameters.md#task_role_arn)\.
+ `task_execution_role` – The name or full ARN of the task execution role\. This is a required field if you want your tasks to be able to store container application logs in CloudWatch or allow your tasks to pull container images from Amazon ECR\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.
+ `task_size` – The CPU and memory values for the task\. If you are using the EC2 launch type, this field is optional and any value can be used\. If using the Fargate launch type, this field is required and you must use one of the following sets of values for the `cpu` and `memory` parameters\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/cmd-ecs-cli-compose-ecsparams.html)

  For more information, see [Task Size](task_definition_parameters.md#task_size)\.
+ `services` – Corresponds to the services listed in your Docker compose file, with `service_name` matching the name of the container to run\. Its fields are merged into a container definition\.
  + `essential` – If the `essential` parameter of a container is marked as `true`, and that container fails or stops for any reason, all other containers that are part of the task are stopped\. If the `essential` parameter of a container is marked as `false`, then its failure does not affect the rest of the containers in a task\. The default value is `true`\.

    All tasks must have at least one essential container\. If you have an application that is composed of multiple containers, you should group containers that are used for a common purpose into components, and separate the different components into multiple task definitions\.
  + `repository_credentials` – If you are using a private repository for pulling images, `repository_credentials` allows you to specify an AWS Secrets Manager secret ARN for the name of the secret containing your private repository credentials as a `credential_parameter`\. For more information, see [Private Registry Authentication for Tasks](private-auth.md)\.
  + `cpu_shares` – This parameter maps to `cpu_shares` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `mem_limit` – This parameter maps to `mem_limit` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `mem_reservation` – This parameter maps to `mem_reservation` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources)\. If you are using Docker compose version 3, this field is optional and must be specified in the ECS params file rather than the compose file\. In Docker compose version 2, this field can be specified in either the compose or ECS params file\. If it is specified in the ECS params file, the value overrides the value present in the compose file\.
  + `healthcheck` – This parameter maps to `healthcheck` in the [Docker compose file reference](https://docs.docker.com/compose/compose-file/#healthcheck)\. The `test` field can also be specified as `command` and must be either a string or a list\. If it's a list, the first item must be either `NONE`, `CMD`, or `CMD-SHELL`\. If it's a string, it's equivalent to specifying `CMD-SHELL` followed by that string\. The `interval`, `timeout`, and `start_period` fields are specified as durations in a string format\. For example: `2.5s`, `10s`, `1m30s`, `2h23m`, or `5h34m56s`\.
**Note**  
If no units are specified, seconds are assumed\. For example, you can specify either `10s` or simply `10`\.
+ `docker_volumes` – This parameter allows you to create docker volumes\. The `name` key is required, and `scope`, `autoprovision`, `driver`, `driver_opts` and `labels` correspond with the Docker volume configuration fields in a task definition\. For more information, see [DockerVolumeConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DockerVolumeConfiguration.html) in the *Amazon Elastic Container Service API Reference*\. Volumes defined with the `docker_volumes` key can be referenced in your compose file by name, even if they were not also specified in the compose file\.

The fields listed under `run_params` are for values needed as options to any API calls not specifically related to a task definition, such as `compose up` \(RunTask\) and `compose service up` \(CreateService\)\. Currently, the only supported parameter under `run_params` is `network_configuration`, which is a required parameter to use task networking and when using tasks with the Fargate launch type\.
+ `network_configuration` – Required if you specified `awsvpc` for `ecs_network_mode`\. It uses one nested parameter, `awsvpc_configuration`, which has the following subfields:
  + `subnets` – A list of subnet IDs used to associate with your tasks\. The listed subnets must be in the same VPC and Availability Zone as the instances on which to launch your tasks\.
  + `security_groups` – A list of security group IDs to associate with your tasks\. The listed security must be in the same VPC as the instances on which to launch your tasks\.
  + `assign_public_ip` – The supported values for this field are `ENABLED` or `DISABLED`\. This field is only used for tasks using the Fargate launch type\. If this field is present in tasks using task networking with the EC2 launch type, the request fails\.
+ `task_placement` – This parameter allows you to specify task placement options\. It is optional if you are using the EC2 launch type\. It is not supported Fargate launch type\. For more information, see [Amazon ECS Task Placement](task-placement.md)\.

  It has the following subfields:
  + `strategy` – A list of objects, with two keys\. Valid keys are `type` and `field`\.
    + `type` – Valid values are `random`, `binpack`, or `spread`\. If `random` is specified, the `field` key should not be provided\.
    + `field` – Valid values depend on the strategy type\.
      + For `spread`, valid values are `instanceId`, `host`, or attribute key\-value pairs, for example `attribute:ecs.instance-type =~ t2.*`\.
      + For `binpack`, valid values are `cpu` or `memory`\.
  + `constraint` – A list of objects, with two keys\. Valid keys are `type` and `expression`\.
    + `type` – Valid values are `distinctInstance` and `memberOf`\. If `distinctInstance` is specified, the `expression` key should not be provided\.
    + `expression` – When type is `memberOf`, valid values are key\-value pairs for attributes or task groups, for example `task:group == databases` or `attribute:color =~ green`\.
+ `service_discovery` – This parameter allows you to configure Amazon ECS Service Discovery using Amazon Route 53 auto naming API actions to manage DNS entries for your service's tasks\. For more information, see [ Service Discovery  Your Amazon ECS service can optionally be configured to use Amazon ECS Service Discovery\. Service discovery uses Amazon Route 53 auto naming API actions to manage DNS entries for your service's tasks, making them discoverable within your VPC\. For more information, see [Using Auto Naming for Service Discovery](https://docs.aws.amazon.com/Route53/latest/APIReference/overview-service-discovery.html) in the *Amazon Route 53 API Reference*\. Service discovery is available in the following AWS Regions: 


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US East \(Ohio\) | us\-east\-2 | 
| US West \(N\. California\) | us\-west\-1 | 
| US West \(Oregon\) | us\-west\-2 | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | 
| EU \(Frankfurt\) | eu\-central\-1 | 
| EU \(Ireland\) | eu\-west\-1 | 
| EU \(London\) | eu\-west\-2 | 
| EU \(Paris\) | eu\-west\-3 | 
| South America \(São Paulo\) | sa\-east\-1 | 
| Canada \(Central\) | ca\-central\-1 |  Topics  Service Discovery Concepts  Service discovery consists of the following components:   **Service discovery namespace**: A logical group of services that share the same domain name, such as example\.com\. You need one namespace per Route 53 hosted zone and per VPC\. If you are using service discovery from the Amazon ECS console, the workflow creates one private namespace per ECS cluster\.    **Service discovery service**: Exists within the service discovery namespace and consists of the service name and DNS configuration for the namespace\. It provides the following core component:   **Service directory**: Allows you to look up a service via DNS or Route 53 auto naming API actions and get back one or more available endpoints that can be used to connect to the service\.     **Health checks**: Perform periodic container\-level health checks\. If an endpoint does not pass the health check, it is removed from DNS routing and marked as unhealthy\. For more information, see [How Amazon Route 53 Checks the Health of Your Resources](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome-health-checks.html)\.     Service Discovery Considerations  The following should be considered when using service discovery:   Service discovery is supported for Fargate tasks if using platform version v1\.1\.0 or later\. For more information, see [AWS Fargate Platform Versions](platform_versions.md)\.   Public namespaces are supported but you must have an existing public hosted zone registered with Route 53 before creating your service discovery service\.   The DNS records created for a service discovery service will always register with the private IP address for the task, rather than the public IP address, even when public namespaces are used\.   Service discovery requires that tasks specify either the `awsvpc`, `bridge`, or `host` network mode \(`none` is not supported\)\.   If the task definition your service task specifies uses the `awsvpc` network mode, you can create any combination of A or SRV records for each service task\. If you use SRV records, a port is required\.   If the task definition that your service task specifies uses the `bridge` or `host` network mode, a SRV record is the only supported DNS record type\. Create a SRV record for each service task\. The SRV record must specify a container name and container port combination from the task definition\.   DNS records for a service discovery service can be queried within your VPC\. They use the following format: `<service discovery service name>.<service discovery namespace>`\. For more information, see [Step 5: Verify the service discovery](create-service-discovery.md#create-service-discovery-verify)\.   When doing a DNS query on the service name, A records return a set of IP addresses that correspond to your tasks\. SRV records return a set of IP addresses and ports per task\.   You can configure service discovery for an ECS service that is behind a load balancer, but service discovery traffic is always routed to the task and not the load balancer\.   Service discovery does not support the use of Classic Load Balancers\.   When specifying health checks for your service discovery service, you must use either custom health checks managed by Amazon ECS or Route 53 health checks\. The two options for health checks cannot be combined\.   **HealthCheckCustomConfig**—Amazon ECS manages health checks on your behalf\. Amazon ECS uses information from container and Elastic Load Balancing health checks, as well as your task state, to update the health with Route 53\. This is specified using the `--health-check-custom-config` parameter when creating your service discovery service\. For more information, see [HealthCheckCustomConfig](https://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_HealthCheckCustomConfig.html) in the *Amazon Route 53 API Reference*\.   **HealthCheckConfig**—Route 53 creates health checks to monitor tasks\. This requires the tasks to be publicly available\. This is specified using the `--health-check-config` parameter when creating your service discovery service\. For more information, see [HealthCheckConfig](https://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_HealthCheckConfig.html) in the *Amazon Route 53 API Reference*\.     If you are using the Amazon ECS console, the workflow creates one service discovery service per ECS service and maps all of the task IP addresses as A records, or task IP addresses and port as SRV records\.   Service discovery can only be configured when first creating a service\. Updating existing services to configure service discovery for the first time or change the current configuration is not supported\.   The Route 53 resources created when service discovery is used must be cleaned up manually\. For more information, see [Step 6: Clean Up](create-service-discovery.md#create-service-discovery-cleanup) in the [Tutorial: Creating a Service Using Service Discovery](create-service-discovery.md) topic\.     Service Discovery Pricing  Customers using Amazon ECS service discovery are charged for the usage of Route 53 auto naming APIs\. This involves costs for creating the hosted zones and queries to the service registry\. For more information, see [Amazon Route 53 Pricing](https://aws.amazon.com/route53/pricing/) Amazon ECS performs container level health checks and exposes this to Route 53 custom health check APIs and this is currently made available to customers at no extra cost\. If you configure additional network health checks for publicly exposed tasks, you are charged for those health checks\.    Tutorial: Creating a Service Using Service Discovery  Service discovery has been integrated into the Create Service wizard in the Amazon ECS console\. For more information, see [Creating a Service](create-service.md)\. The following tutorial shows how to create an ECS service containing a Fargate task that uses service discovery with the AWS CLI\.  Fargate tasks are only supported in the following regions: 


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US East \(Ohio\) | us\-east\-2 | 
| US West \(Oregon\) | us\-west\-2 | 
| EU \(Ireland\) | eu\-west\-1 | 
| EU \(Frankfurt\) | eu\-central\-1 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 |   [Prerequisites](#create-service-discovery-prereqs) [Step 1: Create the Service Discovery Namespace and Service ](#create-service-discovery-namespace) [Step 2: Create a Cluster](#create-service-discovery-cluster) [Step 3: Register a Task Definition](#create-service-discovery-taskdef) [Step 4: Create a Service](#create-service-discovery-service) [Step 5: Verify the service discovery](#create-service-discovery-verify) [Step 6: Clean Up](#create-service-discovery-cleanup)  Prerequisites  This tutorial assumes that the following prerequisites have been completed:   The latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading your AWS CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.   The steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.   Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard](IAMPolicyExamples.md#first-run-permissions) IAM policy example\.   You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.     Step 1: Create the Service Discovery Namespace and Service   Create a private service discovery namespace named `tutorial` within an existing VPC: 

```
aws servicediscovery create-private-dns-namespace --name tutorial --vpc vpc-abcd1234 --region us-east-1
``` Output: 

```
{
    "OperationId": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
}
``` Using the `OperationId` from the previous output, verify that the private namespace was created successfully\. Copy the namespace ID as it is used in subsequent commands\. 

```
aws servicediscovery get-operation --operation-id h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e
``` Output: 

```
{
    "Operation": {
        "Id": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e",
        "Type": "CREATE_NAMESPACE",
        "Status": "SUCCESS",
        "CreateDate": 1519777852.502,
        "UpdateDate": 1519777856.086,
        "Targets": {
            "NAMESPACE": "ns-uejictsjen2i4eeg"
        }
    }
}
``` Using the `NAMESPACE` ID from the previous output, create a service discovery service named `myapplication`\. Copy the service discovery service ID as it is used in subsequent commands\. 

```
aws servicediscovery create-service --name myapplication --dns-config 'NamespaceId="ns-uejictsjen2i4eeg",DnsRecords=[{Type="A",TTL="300"}]' --health-check-custom-config FailureThreshold=1 --region us-east-1
``` Output: 

```
{
    "Service": {
        "Id": "srv-utcrh6wavdkggqtk",
        "Arn": "arn:aws:servicediscovery:region:aws_account_id:service/srv-utcrh6wavdkggqtk",
        "Name": "myapplication",
        "DnsConfig": {
            "NamespaceId": "ns-uejictsjen2i4eeg",
            "DnsRecords": [
                {
                    "Type": "A",
                    "TTL": 300
                }
            ]
        },
        "HealthCheckCustomConfig": {
            "FailureThreshold": 1
        },
        "CreatorRequestId": "e49a8797-b735-481b-a657-b74d1d6734eb"
    }
}
```   Step 2: Create a Cluster  Create an ECS cluster named `tutorial` to use\. 

```
aws ecs create-cluster --cluster-name tutorial --region us-east-1
``` Output: 

```
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/tutorial",
        "clusterName": "tutorial",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": []
    }
}
```   Step 3: Register a Task Definition  Register a task definition that is compatible with Fargate\. It requires the use of the `awsvpc` network mode\. The following is the example task definition used for this tutorial\. First, create a file named `fargate-task.json` with the contents of the following task definition: 

```
{
    "family": "tutorial-task-def",
        "networkMode": "awsvpc",
        "containerDefinitions": [
            {
                "name": "sample-app",
                "image": "httpd:2.4",
                "portMappings": [
                    {
                        "containerPort": 80,
                        "hostPort": 80,
                        "protocol": "tcp"
                    }
                ],
                "essential": true,
                "entryPoint": [
                    "sh",
                    "-c"
                ],
                "command": [
                    "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
                ]
            }
        ],
        "requiresCompatibilities": [
            "FARGATE"
        ],
        "cpu": "256",
        "memory": "512"
}
``` Then, register the task definition using the `fargate-task.json` file you created\. 

```
aws ecs register-task-definition --cli-input-json file://fargate-task.json --region us-east-1
```   Step 4: Create a Service  Create a file named `ecs-service-discovery.json` with the contents of the ECS service that you are going to create\. This example uses the task definition created in the previous step\. An `awsvpcConfiguration` is required because the example task definition uses the `awsvpc` network mode\. 

```
{
    "cluster": "tutorial",
    "serviceName": "ecs-service-discovery",
    "taskDefinition": "tutorial-task-def",
    "serviceRegistries": [
       {
          "registryArn": "arn:aws:servicediscovery:region:aws_account_id:service/srv-utcrh6wavdkggqtk"
       }
    ],
    "launchType": "FARGATE",
    "platformVersion": "1.1.0",
    "networkConfiguration": {
       "awsvpcConfiguration": {
          "assignPublicIp": "ENABLED",
          "securityGroups": [ "sg-abcd1234" ],
          "subnets": [ "subnet-abcd1234" ]
       }
    },
    "desiredCount": 1
}
``` Create your ECS service, specifying the Fargate launch type and the `1.1.0` platform version, which uses service discovery\. 

```
aws ecs create-service --cli-input-json file://ecs-service-discovery.json --region us-east-1
``` Output: 

```
{
    "service": {
        "serviceArn": "arn:aws:ecs:region:aws_account_id:service/ecs-service-discovery",
        "serviceName": "ecs-service-discovery",
        "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/tutorial",
        "loadBalancers": [],
        "serviceRegistries": [
            {
                "registryArn": "arn:aws:servicediscovery:region:aws_account_id:service/srv-utcrh6wavdkggqtk"
            }
        ],
        "status": "ACTIVE",
        "desiredCount": 1,
        "runningCount": 0,
        "pendingCount": 0,
        "launchType": "FARGATE",
        "platformVersion": "1.1.0",
        "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/tutorial-task-def:1",
        "deploymentConfiguration": {
            "maximumPercent": 200,
            "minimumHealthyPercent": 100
        },
        "deployments": [
            {
                "id": "ecs-svc/9223370516993140842",
                "status": "PRIMARY",
                "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/tutorial-task-def:1",
                "desiredCount": 1,
                "pendingCount": 0,
                "runningCount": 0,
                "createdAt": 1519861634.965,
                "updatedAt": 1519861634.965,
                "launchType": "FARGATE",
                "platformVersion": "1.1.0",
                "networkConfiguration": {
                    "awsvpcConfiguration": {
                        "subnets": [
                            "subnet-abcd1234"
                        ],
                        "securityGroups": [
                            "sg-abcd1234"
                        ],
                        "assignPublicIp": "ENABLED"
                    }
                }
            }
        ],
        "roleArn": "arn:aws:iam::aws_account_id:role/ECSServiceLinkedRole",
        "events": [],
        "createdAt": 1519861634.965,
        "placementConstraints": [],
        "placementStrategy": [],
        "networkConfiguration": {
            "awsvpcConfiguration": {
                "subnets": [
                    "subnet-abcd1234"
                ],
                "securityGroups": [
                    "sg-abcd1234"
                ],
                "assignPublicIp": "ENABLED"
            }
        }
    }
}
```   Step 5: Verify the service discovery  You can verify that everything has been created properly by querying your DNS information\. After service discovery is configured, you can query it using either the Route 53 auto naming API actions or by using `dig` from within your VPC, as described below\. Using the service discovery service ID, list the service discovery instances: 

```
aws servicediscovery list-instances --service-id srv-utcrh6wavdkggqtk --region us-east-1
``` Output: 

```
{
    "Instances": [
        {
            "Id": "16becc26-8558-4af1-9fbd-f81be062a266",
            "Attributes": {
                "AWS_INSTANCE_IPV4": "172.31.87.2"
            }
        }
    ]
}
``` The DNS records created in the Route 53 hosted zone for the service discovery service can be queried with the following CLI commands\. First, using the namespace ID, get information about the namespace, which includes the Route 53 hosted zone ID: 

```
aws servicediscovery get-namespace --id ns-uejictsjen2i4eeg --region us-east-1
``` Output: 

```
{
    "Namespace": {
        "Id": "ns-uejictsjen2i4eeg",
        "Arn": "arn:aws:servicediscovery:region:aws_account_id:namespace/ns-uejictsjen2i4eeg",
        "Name": "tutorial",
        "Type": "DNS_PRIVATE",
        "Properties": {
            "DnsProperties": {
                "HostedZoneId": "Z35JQ4ZFDRYPLV"
            }
        },
        "CreateDate": 1519777852.502,
        "CreatorRequestId": "9049a1d5-25e4-4115-8625-96dbda9a6093"
    }
}
``` Then, using the Route 53 hosted zone ID, get the resource record set for the hosted zone: 

```
aws route53 list-resource-record-sets --hosted-zone-id Z35JQ4ZFDRYPLV --region us-east-1
``` Output: 

```
{
    "ResourceRecordSets": [
        {
            "Name": "tutorial.",
            "Type": "NS",
            "TTL": 172800,
            "ResourceRecords": [
                {
                    "Value": "ns-1536.awsdns-00.co.uk."
                },
                {
                    "Value": "ns-0.awsdns-00.com."
                },
                {
                    "Value": "ns-1024.awsdns-00.org."
                },
                {
                    "Value": "ns-512.awsdns-00.net."
                }
            ]
        },
        {
            "Name": "tutorial.",
            "Type": "SOA",
            "TTL": 900,
            "ResourceRecords": [
                {
                    "Value": "ns-1536.awsdns-00.co.uk. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400"
                }
            ]
        },
        {
            "Name": "myapplication.tutorial.",
            "Type": "A",
            "SetIdentifier": "16becc26-8558-4af1-9fbd-f81be062a266",
            "MultiValueAnswer": true,
            "TTL": 300,
            "ResourceRecords": [
                {
                    "Value": "172.31.87.2"
                }
            ]
        }
    ]
}
``` You can also query the DNS using `dig` from an instance within your VPC with the following command: 

```
dig +short myapplication.tutorial
``` Output: 

```
172.31.87.2
```   Step 6: Clean Up  When you are finished with this tutorial, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\. Cleaning up the service discovery and Amazon ECS resources  Deregister the service discovery service instances\. 

   ```
   aws servicediscovery deregister-instance --service-id srv-utcrh6wavdkggqtk --instance-id 16becc26-8558-4af1-9fbd-f81be062a266 --region us-east-1
   ``` Output: 

   ```
   {
       "OperationId": "xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv"
   }
   ```   Using the `OperationId` from the previous output, verify that the service discovery service instances were deregistered successfully\. 

   ```
   aws servicediscovery get-operation --operation-id xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv --region us-east-1
   ``` Output: 

   ```
   {
       "Operation": {
           "Id": "xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv",
           "Type": "DEREGISTER_INSTANCE",
           "Status": "SUCCESS",
           "CreateDate": 1525984073.707,
           "UpdateDate": 1525984076.426,
           "Targets": {
               "INSTANCE": "16becc26-8558-4af1-9fbd-f81be062a266",
               "ROUTE_53_CHANGE_ID": "C5NSRG1J4I1FH",
               "SERVICE": "srv-utcrh6wavdkggqtk"
           }
       }
   }
   ```   Delete the service discovery service\. 

   ```
   aws servicediscovery delete-service --id srv-utcrh6wavdkggqtk --region us-east-1
   ```   Delete the service discovery namespace\. 

   ```
   aws servicediscovery delete-namespace --id ns-uejictsjen2i4eeg --region us-east-1
   ``` Output: 

   ```
   {
       "OperationId": "c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj"
   }
   ```   Using the `OperationId` from the previous output, verify that the service discovery namespace was deleted successfully\. 

   ```
   aws servicediscovery get-operation --operation-id c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj --region us-east-1
   ``` Output: 

   ```
   {
       "Operation": {
           "Id": "c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj",
           "Type": "DELETE_NAMESPACE",
           "Status": "SUCCESS",
           "CreateDate": 1525984602.211,
           "UpdateDate": 1525984602.558,
           "Targets": {
               "NAMESPACE": "ns-rymlehshst7hhukh",
               "ROUTE_53_CHANGE_ID": "CJP2A2M86XW3O"
           }
       }
   }
   ```   Update the Amazon ECS service so the desired count is `0`, which allows you to delete it\. 

   ```
   aws ecs update-service --cluster tutorial --service ecs-service-discovery --desired-count 0 --force-new-deployment --region us-east-1
   ```   Delete the Amazon ECS service\. 

   ```
   aws ecs delete-service --cluster tutorial --service ecs-service-discovery --region us-east-1
   ```   Delete the Amazon ECS cluster\. 

   ```
   aws ecs delete-cluster --cluster tutorial --region us-east-1
   ```     ](service-discovery.md)\.