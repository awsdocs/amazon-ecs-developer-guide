# Service Discovery<a name="service-discovery"></a>

Your Amazon ECS service can optionally be configured to use Amazon ECS Service Discovery\. Service discovery uses Amazon Route 53 auto naming APIs to manage DNS entries for your service's tasks, making them discoverable within your VPC\. For more information, see [Using Auto Naming for Service Discovery](http://docs.aws.amazon.com/Route53/latest/APIReference/overview-service-discovery.html) in the *Amazon Route 53 API Reference*\.

**Note**  
Service discovery is available in the following Regions:  


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US East \(Ohio\) | us\-east\-2 | 
| US West \(Oregon\) | us\-west\-2 | 
| EU \(Ireland\) | eu\-west\-1 | 

**Topics**
+ [Service Discovery Concepts](#service-discovery-concepts)
+ [Service Discovery Considerations](#service-discovery-considerations)
+ [Service Discovery Pricing](#service-discovery-pricing)
+ [Creating a Service Using Service Discovery](#create-service-discovery)

## Service Discovery Concepts<a name="service-discovery-concepts"></a>

Service discovery consists of the following components:
+ **Service discovery namespace**: A logical group of services that share the same domain name, such as example\.com\. You need one namespace per Route 53 hosted zone and per VPC\. If you are using service discovery from the Amazon ECS console, the workflow creates one private namespace per ECS cluster\. 
+ **Service discovery service**: Exists within the service discovery namespace and consists of the service name and DNS configuration for the namespace\. It provides the following core component:
  + **Service directory**: Allows you to look up a service via DNS or Route 53 auto naming APIs and get back one or more available endpoints that can be used to connect to the service\.
+ **Health checks**: Perform periodic container\-level health checks\. If an endpoint does not pass the health check, it is removed from DNS routing and marked as unhealthy\. For more information, see [How Amazon Route 53 Checks the Health of Your Resources](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome-health-checks.html)\.

## Service Discovery Considerations<a name="service-discovery-considerations"></a>

The following should be considered when using service discovery:
+ Public namespaces are supported but you must have an existing public hosted zone registered with Route 53 before creating your service discovery service\.
+ Service discovery only supports tasks that use the `awsvpc` network mode\.
+ DNS records for a service discovery service can be queried within your VPC\. They use the following format: `<service discovery service name>.<service discovery namespace>`\. For an example, see [Creating a Service Using Service Discovery](#service-discovery-query)\.
+ You can create any combination of A or SRV records for each service task\. If you use SRV records, a port is required\.
+ When doing a DNS query on the service name, A records return a set of IP addresses that correspond to your tasks\. SRV records return a set of IP addresses and ports per task\.
+ If a load balancer is configured for the ECS service and you enable service discovery, an A record is used to route traffic to the load balancer\. The load balancer also handles the health checks\.
+ When specifying health checks for your service discovery service, you must use either custom health checks managed by Amazon ECS or Route 53 health checks\. The two options for health checks cannot be combined\.
  + **HealthCheckCustomConfig**—Amazon ECS manages health checks on your behalf\. Amazon ECS uses information from container and Elastic Load Balancing health checks, as well as your task state, to update the health with Route 53\. This is specified using the `--health-check-custom-config` parameter when creating your service discovery service\. For more information, see [HealthCheckCustomConfig](http://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_HealthCheckCustomConfig.html) in the *Amazon Route 53 API Reference*\.
  + **HealthCheckConfig**—Route 53 creates health checks to monitor tasks\. This requires the tasks to be publicly available\. This is specified using the `--health-check-config` parameter when creating your service discovery service\. For more information, see [HealthCheckConfig](http://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_HealthCheckConfig.html) in the *Amazon Route 53 API Reference*\.
+ If you are using the Amazon ECS console, the workflow creates one service discovery service per ECS service and maps all of the task IP addresses as A records, or task IP addresses and port as SRV records\.
+ Existing ECS services that have service discovery configured cannot be updated to change the service discovery configuration\.

**Note**  
Service discovery is supported for Fargate tasks if using platform version v1\.1\.0 or later\. For more information, see [AWS Fargate Platform Versions](platform_versions.md)\.

## Service Discovery Pricing<a name="service-discovery-pricing"></a>

Customers using Amazon ECS service discovery are charged for the usage of Route 53 auto naming APIs\. This involves costs for creating the hosted zones and queries to the service registry\. For more information, see [Amazon Route 53 Pricing](https://aws.amazon.com/route53/pricing/)

Amazon ECS performs container level health checks and exposes this to Route 53 custom health check APIs and this is currently made available to customers at no extra cost\. If you configure additional network health checks for publicly exposed tasks, you are charged for those health checks\. 

## Creating a Service Using Service Discovery<a name="create-service-discovery"></a>

Service discovery has been integrated into the Create Service wizard in the Amazon ECS console\. For more information, see [Creating a Service](create-service.md)\.

The following tutorial shows how to create an ECS service using a Fargate task that uses service discovery with the AWS CLI\.

**Note**  
Fargate tasks are only supported in the US East \(N\. Virginia\) region\.

This tutorial assumes the following prerequisites have been completed:
+ The latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading your AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ The steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard](IAMPolicyExamples.md#first-run-permissions) IAM policy example\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

**To create a service using service discovery and the AWS CLI**

1. Create a private namespace named `staging` within an existing VPC:

   ```
   aws servicediscovery create-private-dns-namespace --name staging --vpc vpc-abcd1234 --region us-east-1
   ```

   Output:

   ```
   {
       "OperationId": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
   }
   ```

1. Using the `OperationId` from the previous output, verify that the private namespace was created successfully\.

   ```
   aws servicediscovery get-operation --operation-id h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e
   ```

   Output:

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
   ```

1. Create a service discovery service named `myapplication`\.

   ```
   aws servicediscovery create-service --name myapplication --dns-config NamespaceId="ns-uejictsjen2i4eeg",DnsRecords=[{Type="A",TTL="300"}] --health-check-custom-config FailureThreshold=1 --region us-east-1
   ```

   Output:

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
   ```

1. Create an ECS cluster named `staging` to use\.

   ```
   aws ecs create-cluster --cluster-name staging --region us-east-1
   ```

   Output:

   ```
   {
       "cluster": {
           "clusterArn": "arn:aws:ecs:us-east-1:809632081692:cluster/staging",
           "clusterName": "staging",
           "status": "ACTIVE",
           "registeredContainerInstancesCount": 0,
           "runningTasksCount": 0,
           "pendingTasksCount": 0,
           "activeServicesCount": 0,
           "statistics": []
       }
   }
   ```

1. Register a task definition that is compatible with Fargate\. It will require the use of the `awsvpc` network mode\. The following is the example task definition used for this tutorial\.

   Create a file named `fargate-task.json` with the contents of the following task definition:

   ```
   {
       "taskDefinition": {
           "family": "first-run-task-definition",
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
                   ],
               }
           ],
           "requiresCompatibilities": [
               "FARGATE"
           ],
           "cpu": "256",
           "memory": "512"
       }
   }
   ```

   Register the task definition using the `fargate-task.json` file you created\.

   ```
   aws ecs register-task-definition --cli-input-json file://fargate-task.json --region us-east-1
   ```

1. Create a file named `ecs-service-discovery.json` with the contents of the ECS service that you are going to create\. This example uses the task definition created in the previous step\. An `awsvpcConfiguration` is required because the example task definition uses the `awsvpc` network mode\.

   ```
   {
       "cluster": "staging",
       "serviceName": "ecs-service-discovery",
       "taskDefinition": "first-run-task-definition",
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
   ```

1. Create your ECS service, specifying the Fargate launch type and the `1.1.0` platform version, which will use service discovery\.

   ```
   aws ecs create-service --cli-input-json file://ecs-service-discovery.json --region us-east-1
   ```

   Output:

   ```
   {
       "service": {
           "serviceArn": "arn:aws:ecs:region:aws_account_id:service/ecs-service-discovery",
           "serviceName": "ecs-service-discovery",
           "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/default",
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
           "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/first-run-task-definition:4",
           "deploymentConfiguration": {
               "maximumPercent": 200,
               "minimumHealthyPercent": 100
           },
           "deployments": [
               {
                   "id": "ecs-svc/9223370516993140842",
                   "status": "PRIMARY",
                   "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/first-run-task-definition:4",
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
   ```

You can then verify that everything has been created properly and query your DNS information\.

**To verify service discovery configuration**

Once service discovery is configured, you can query it using either the Route 53 Auto Naming APIs or by using `dig` from within your VPC, as described below\.

1. After the ECS service is created and at least one task is successfully running, you can then list the service discovery instances to confirm that it is set up:

   ```
   aws servicediscovery list-instances --service-id srv-utcrh6wavdkggqtk --region us-east-1
   ```

   Output:

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
   ```

1. The DNS records created in the Route 53 hosted zone for your service discovery service can be queried with the following CLI commands\.

   First, get information about your namespace, which includes the Route 53 hosted zone ID:

   ```
   aws servicediscovery get-namespace --id ns-ltr6u2zk2fvhjieu --region us-east-1
   ```

   Output:

   ```
   {
       "Namespace": {
           "Id": "ns-uejictsjen2i4eeg",
           "Arn": "arn:aws:servicediscovery:region:aws_account_id:namespace/ns-uejictsjen2i4eeg",
           "Name": "cli-tutorial",
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
   ```

   Then, get the resource record set for the hosted zone:

   ```
   aws route53 list-resource-record-sets --hosted-zone-id Z35JQ4ZFDRYPLV --region us-east-1
   ```

   Output:

   ```
   {
       "ResourceRecordSets": [
           {
               "Name": "staging.",
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
               "Name": "staging.",
               "Type": "SOA",
               "TTL": 900,
               "ResourceRecords": [
                   {
                       "Value": "ns-1536.awsdns-00.co.uk. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400"
                   }
               ]
           },
           {
               "Name": "myapplication.staging.",
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
   ```

1. <a name="service-discovery-query"></a>You can also query the DNS using `dig` from an instance within your VPC with the following command:

   ```
   dig +short myapplication.staging
   ```

   Output:

   ```
   172.31.87.2
   ```