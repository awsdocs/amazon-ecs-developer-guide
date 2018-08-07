# Service Discovery<a name="service-discovery"></a>

Your Amazon ECS service can optionally be configured to use Amazon ECS Service Discovery\. Service discovery uses Amazon Route 53 auto naming API actions to manage DNS entries for your service's tasks, making them discoverable within your VPC\. For more information, see [Using Auto Naming for Service Discovery](http://docs.aws.amazon.com/Route53/latest/APIReference/overview-service-discovery.html) in the *Amazon Route 53 API Reference*\.

Service discovery is available in the following AWS Regions:


| Region Name | Region | 
| --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US East \(Ohio\) | us\-east\-2 | 
| US West \(N\. California\) | us\-west\-1 | 
| US West \(Oregon\) | us\-west\-2 | 
| EU \(Ireland\) | eu\-west\-1 | 

**Topics**
+ [Service Discovery Concepts](#service-discovery-concepts)
+ [Service Discovery Considerations](#service-discovery-considerations)
+ [Service Discovery Pricing](#service-discovery-pricing)
+ [Tutorial: Creating a Service Using Service Discovery](create-service-discovery.md)

## Service Discovery Concepts<a name="service-discovery-concepts"></a>

Service discovery consists of the following components:
+ **Service discovery namespace**: A logical group of services that share the same domain name, such as example\.com\. You need one namespace per Route 53 hosted zone and per VPC\. If you are using service discovery from the Amazon ECS console, the workflow creates one private namespace per ECS cluster\. 
+ **Service discovery service**: Exists within the service discovery namespace and consists of the service name and DNS configuration for the namespace\. It provides the following core component:
  + **Service directory**: Allows you to look up a service via DNS or Route 53 auto naming API actions and get back one or more available endpoints that can be used to connect to the service\.
+ **Health checks**: Perform periodic container\-level health checks\. If an endpoint does not pass the health check, it is removed from DNS routing and marked as unhealthy\. For more information, see [How Amazon Route 53 Checks the Health of Your Resources](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome-health-checks.html)\.

## Service Discovery Considerations<a name="service-discovery-considerations"></a>

The following should be considered when using service discovery:
+ Public namespaces are supported but you must have an existing public hosted zone registered with Route 53 before creating your service discovery service\.
+ Service discovery requires that tasks specify either the `awsvpc`, `bridge`, or `host` network mode \(`none` is not supported\)\.
+ If the task definition your service task specifies uses the `awsvpc` network mode, you can create any combination of A or SRV records for each service task\. If you use SRV records, a port is required\.
+ If the task definition that your service task specifies uses the `bridge` or `host` network mode, a SRV record is the only supported DNS record type\. Create a SRV record for each service task\. The SRV record must specify a container name and container port combination from the task definition\.
+ DNS records for a service discovery service can be queried within your VPC\. They use the following format: `<service discovery service name>.<service discovery namespace>`\. For more information, see [Step 5: Verify the service discovery](create-service-discovery.md#create-service-discovery-verify)\.
+ When doing a DNS query on the service name, A records return a set of IP addresses that correspond to your tasks\. SRV records return a set of IP addresses and ports per task\.
+ You can configure service discovery for an ECS service that is behind a load balancer, but service discovery traffic is always routed to the task and not the load balancer\.
+ Service discovery does not support the use of Classic Load Balancers\.
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