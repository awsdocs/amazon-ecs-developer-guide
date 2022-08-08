# Service Discovery<a name="service-discovery"></a>

Amazon ECS services can be configured to use Service Discovery\. Service discovery uses AWS Cloud Map API actions to manage HTTP and DNS namespaces for your Amazon ECS services\. For more information, see [What Is AWS Cloud Map?](https://docs.aws.amazon.com/cloud-map/latest/dg/Welcome.html) in the *AWS Cloud Map Developer Guide*\. 

When you create a private namespace in AWS Cloud Map, we automatically create a Route 53 hosted zone\. The Amazon ECS service can register with a friendly and predictable DNS name in Route 53\. The hosted zones are updated automatically as your Amazon ECS services scale up or down\. Other services perform a DNS lookup to determine whether to make a connection to the service\.

Service discovery is available in the following AWS Regions:


| Region Name | Region | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US West \(N\. California\)  |  us\-west\-1  | 
|  US West \(Oregon\)  |  us\-west\-2  | 
|  Africa \(Cape Town\)  |  af\-south\-1  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | 
|  Asia Pacific \(Jakarta\)  |  ap\-southeast\-3  | 
|  Canada \(Central\)  |  ca\-central\-1  | 
|  China \(Beijing\)  |  cn\-north\-1  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | 
|  Europe \(London\)  |  eu\-west\-2  | 
|  Europe \(Paris\)  |  eu\-west\-3  | 
|  Europe \(Milan\)  |  eu\-south\-1  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  | 
|  South America \(São Paulo\)  |  sa\-east\-1  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  | 

## Service Discovery concepts<a name="service-discovery-concepts"></a>

Service discovery consists of the following components:
+ **Service discovery namespace**: A logical group of service discovery services that share the same domain name, such as `example.com`\. This is the domain name where you want to route traffic to\. You can create a namespace with a call to the `aws servicediscovery create-private-dns-namespace` command or in the Amazon ECS classic console\. You can use the `aws servicediscovery list-namespaces` command to view the summary information about the namespaces that were created by the current account\. For more information about the service discovery commands, see `[create\-private\-dns\-namespace](https://docs.aws.amazon.com/cli/latest/reference/servicediscovery/create-private-dns-namespace.html)` and `[list\-namespaces](https://docs.aws.amazon.com/cli/latest/reference/servicediscovery/list-namespaces.html)` in the *AWS Cloud Map \(service discovery\) AWS CLI Reference Guide*\.
+ **Service discovery service**: Exists within the service discovery namespace and consists of the service name and DNS configuration for the namespace\. It provides the following core component:
  + **Service registry**: Allows you to look up a service via DNS or AWS Cloud Map API actions and get back one or more available endpoints that can be used to connect to the service\.
+ **Service discovery instance**: Exists within the service discovery service and consists of the attributes that are associated with each Amazon ECS service in the service directory\.
  + **Instance attributes**: The following metadata is added as custom attributes for each Amazon ECS service that's configured to use service discovery:
    + **`AWS_INSTANCE_IPV4`** – For an `A` record, the IPv4 address that Route 53 returns in response to DNS queries and AWS Cloud Map returns when discovering instance details, for example, `192.0.2.44`\.
    + **`AWS_INSTANCE_PORT`** – The port value that's associated with the service discovery service\.
    + **`AVAILABILITY_ZONE`** – The Availability Zone that the task was launched into\. For tasks that use the EC2 launch type, this is the Availability Zone where the container instance is in\. For tasks that use the Fargate launch type, this is the Availability Zone where the elastic network interface is in\.
    + **`REGION`** – The Region where the task is in\.
    + **`ECS_SERVICE_NAME`** – The name of the Amazon ECS service that the task belongs to\.
    + **`ECS_CLUSTER_NAME`** – The name of the Amazon ECS cluster that the task belongs to\.
    + **`EC2_INSTANCE_ID`** – The ID of the container instance the task was placed on\. This custom attribute isn't added if the task is using the Fargate launch type\.
    + **`ECS_TASK_DEFINITION_FAMILY`** – The task definition family that the task is using\.
    + **`ECS_TASK_SET_EXTERNAL_ID`** – If a task set is created for an external deployment and is associated with a service discovery registry, then the `ECS_TASK_SET_EXTERNAL_ID` attribute contains the external ID of the task set\.
+ **Amazon ECS health checks**: Amazon ECS performs periodic container\-level health checks\. If an endpoint doesn't pass the health check, it's removed from DNS routing and marked as unhealthy\.

## Service discovery considerations<a name="service-discovery-considerations"></a>

The following should be considered when using service discovery:
+ Service discovery is supported for tasks on Fargate that use platform version 1\.1\.0 or later\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.
+ Services configured to use service discovery have a limit of 1,000 tasks for each service\. This is due to a Route 53 service quota\.
+ The Create Service workflow in the Amazon ECS classic console only supports registering services into private DNS namespaces\. When an AWS Cloud Map private DNS namespace is created, a Route 53 private hosted zone will be created automatically\.
+ The VPC DNS attributes must be configured for successful DNS resolution\. For information about how to configure the attributes, see [DNS support in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-support) in the *Amazon VPC User Guide*\.
+ The DNS records created for a service discovery service always register with the private IP address for the task, rather than the public IP address, even when public namespaces are used\.
+ Service discovery requires that tasks specify either the `awsvpc`, `bridge`, or `host` network mode \(`none` is not supported\)\.
+ If the service task definition uses the `awsvpc` network mode, you can create any combination of `A` or SRV records for each service task\. If you use SRV records, a port is required\.
+ If the service task definition uses the `bridge` or `host` network mode, the SRV record is the only supported DNS record type\. Create a SRV record for each service task\. The SRV record must specify a container name and container port combination from the task definition\.
+ DNS records for a service discovery service can be queried within your VPC\. They use the following format: `<service discovery service name>.<service discovery namespace>`\.
+ When doing a DNS query on the service name, `A` records return a set of IP addresses that correspond to your tasks\. SRV records return a set of IP addresses and ports for each task\.
+ If you have eight or fewer healthy records, Route 53 responds to all DNS queries with all of the healthy records\.
+ When all records are unhealthy, Route 53 responds to DNS queries with up to eight unhealthy records\.
+ You can configure service discovery for an ECS service that's behind a load balancer, but service discovery traffic is always routed to the task and not the load balancer\.
+ Service discovery doesn't support the use of Classic Load Balancers\.
+ We recommend you use container\-level health checks managed by Amazon ECS for your service discovery service\.
  + **HealthCheckCustomConfig**—Amazon ECS manages health checks on your behalf\. Amazon ECS uses information from container and health checks, and your task state, to update the health with AWS Cloud Map\. This is specified using the `--health-check-custom-config` parameter when creating your service discovery service\. For more information, see [HealthCheckCustomConfig](https://docs.aws.amazon.com/cloud-map/latest/api/API_HealthCheckCustomConfig.html) in the *AWS Cloud Map API Reference*\.
+ If you're using the Amazon ECS classic console, the workflow creates one service discovery service for each ECS service\. It maps all of the task IP addresses as `A` records, or task IP addresses and port as SRV records\.
+ Service discovery can only be configured when first creating a service\. Updating existing services to configure service discovery for the first time or change the current configuration isn't supported\.
+ The AWS Cloud Map resources created when service discovery is used must be cleaned up manually\.

## Amazon ECS classic console experience<a name="service-discovery-console"></a>

The workflow to create a service in the Amazon ECS classic console supports service discovery\. Service discovery can only be configured when first creating a service\. Updating existing services to configure service discovery for the first time or change the current configuration isn't supported\.

To create a new Amazon ECS service that uses service discovery, see [Creating an Amazon ECS service](create-service.md)\.

## Service discovery pricing<a name="service-discovery-pricing"></a>

Customers using Amazon ECS service discovery are charged for Route 53 resources and AWS Cloud Map discovery API operations\. This involves costs for creating the Route 53 hosted zones and queries to the service registry\. For more information, see [AWS Cloud Map Pricing](https://docs.aws.amazon.com/cloud-map/latest/dg/cloud-map-pricing.html) in the *AWS Cloud Map Developer Guide*\.

Amazon ECS performs container level health checks and exposes them to AWS Cloud Map custom health check API operations\. This is currently made available to customers at no extra cost\. If you configure additional network health checks for publicly exposed tasks, you're charged for those health checks\.