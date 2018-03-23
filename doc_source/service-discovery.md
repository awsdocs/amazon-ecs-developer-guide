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
+ **Health checks**: Perform periodic container\-level health checks\. If an endpoint does not pass the health check, it is removed from DNS routing and marked as unhealthy\.

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
Service discovery is not supported yet for services that contain Fargate tasks\. This support will be added in an upcoming Fargate platform version\. For more information, see [AWS Fargate Platform Versions](platform_versions.md)\.

## Service Discovery Pricing<a name="service-discovery-pricing"></a>

Customers using Amazon ECS service discovery are charged for the usage of Route 53 auto naming APIs\. This involves costs for creating the hosted zones and queries to the service registry\. For more information, see [Amazon Route 53 Pricing](https://aws.amazon.com/route53/pricing/)

Amazon ECS performs container level health checks and exposes this to Route 53 custom health check APIs and this is currently made available to customers at no extra cost\. If you configure additional network health checks for publicly exposed tasks, you are charged for those health checks\. 

## Creating a Service Using Service Discovery<a name="create-service-discovery"></a>

Service discovery has been integrated into the Create Service wizard in the Amazon ECS console\. For more information, see [Creating a Service](create-service.md)\.

The following is a tutorial showing how to create an ECS service using service discovery and the AWS CLI:

**To create a service using service discovery and the AWS CLI**

1. Create a private namespace within an existing VPC:

   ```
   aws servicediscovery create-private-dns-namespace --name cli-tutorial --vpc "vpc-abcd1234"
   ```

   Output:

   ```
   {
       "OperationId": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
   }
   ```

1. Using the `OperationId` from the previous output, verify that the private namespace was created successfully\.

   ```
   aws servicediscovery get-operation --operation-id "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
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

1. Create a service discovery service using the private namespace ID\.

   ```
   aws servicediscovery create-service --name ecs-service-discovery --dns-config "NamespaceId="ns-uejictsjen2i4eeg",DnsRecords=[{Type="A",TTL="300"}]" --health-check-custom-config "FailureThreshold=1"
   ```

   Output:

   ```
   {
       "Service": {
           "Id": "srv-utcrh6wavdkggqtk",
           "Arn": "arn:aws:servicediscovery:region:aws_account_id:service/srv-utcrh6wavdkggqtk",
           "Name": "clitesting",
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

1. Create a file named `service-discovery.json` with the contents of the ECS service that you are going to create\. In this example, you use a task definition named `first-run-task-definition` and create a service using EC2 tasks\. An `awsvpcConfiguration` is required\. For more information about creating a EC2 task, including creating a task definition, see [Using the AWS CLI with Amazon ECS](ECS_AWSCLI.md)\.

   ```
   {
       "serviceName": "ecs-service-discovery",
       "taskDefinition": "first-run-task-definition",
       "serviceRegistries": [
          {
             "registryArn": "arn:aws:servicediscovery:region:aws_account_id:service/srv-utcrh6wavdkggqtk"
          }
       ],
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

1. Create your ECS service using service discovery:

   ```
   aws ecs create-service --service-name ecs-service-discovery --launch-type EC2 --cli-input-json file://ecs-service-discovery.json
   ```

You can then verify that everything has been created properly and query your DNS information\.

**To verify service discovery configuration**

Once service discovery is configured, you can query it using either the Route 53 Auto Naming APIs or by using `dig` from within your VPC, as described below\.

1. After the ECS service is created and at least one task is successfully running, you can then list the service discovery instances to confirm that it is set up:

   ```
   aws servicediscovery list-instances --service-id "srv-utcrh6wavdkggqtk"
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
   aws servicediscovery get-namespace --id "ns-ltr6u2zk2fvhjieu"
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
   aws route53 list-resource-record-sets --hosted-zone-id "Z35JQ4ZFDRYPLV"
   ```

   Output:

   ```
   {
       "ResourceRecordSets": [
           {
               "Name": "cli-tutorial.",
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
               "Name": "cli-tutorial.",
               "Type": "SOA",
               "TTL": 900,
               "ResourceRecords": [
                   {
                       "Value": "ns-1536.awsdns-00.co.uk. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400"
                   }
               ]
           },
           {
               "Name": "ecs-service-discovery.cli-tutorial.",
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
   dig +short ecs-service-discovery.cli-tutorial
   ```

   Output:

   ```
   172.31.87.2
   ```