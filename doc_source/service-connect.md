# Service Connect<a name="service-connect"></a>

Amazon ECS Service Connect provides management of service\-to\-service communication as Amazon ECS configuration\. It does this by building both service discovery and a service mesh in Amazon ECS\. This provides  the complete configuration inside each Amazon ECS service that you manage by service deployments, a unified way to refer to your services within namespaces that doesn't depend on the Amazon VPC DNS configuration, and standardized metrics and logs to monitor all of your applications on Amazon ECS\. Amazon ECS Service Connect only interconnects Amazon ECS services\.

**Overview of steps to configure Service Connect**  
Follow these steps to configure Service Connect for a group of related services\.

1. Configure port names in your task definitions\. Additionally, you can identify the layer 7 protocol of the application, to get additional metrics\.

1. Create a AWS Cloud Map namespace\. For simple organization, create an Amazon ECS cluster and specify the identical name for the namespace\. In this case, Amazon ECS creates a new HTTP namespace with the necessary configuration\. Amazon ECS Service Connect doesn't use or create DNS hosted zones in Amazon Route 53\.

1. Configure services to create Service Connect endpoints within the namespace\.

1. Deploy services to create the endpoints\. Amazon ECS adds a Service Connect proxy container to each task, and creates the Service Connect endpoints in AWS Cloud Map\. This container isn't configured in the task definition, and the task definition can be reused without modification to create multiple services in the same namespace or in multiple namespaces\.

1. Deploy client apps as services to connect to the endpoints\. Amazon ECS connects them to the Service Connect endpoints through the Service Connect proxy in each task\.

1. Monitor traffic through the Service Connect proxy in Amazon CloudWatch\.

Amazon ECS Service Connect is available in the following AWS Regions:


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
|  Asia Pacific \(Osaka\)  |  ap\-northeast\-3  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | 
|  Asia Pacific \(Jakarta\)  |  ap\-southeast\-3  | 
|  Canada \(Central\)  |  ca\-central\-1  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | 
|  Europe \(London\)  |  eu\-west\-2  | 
|  Europe \(Paris\)  |  eu\-west\-3  | 
|  Europe \(Milan\)  |  eu\-south\-1  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  | 
|  South America \(São Paulo\)  |  sa\-east\-1  | 

## Service Connect concepts<a name="service-connect-concepts"></a>

### <a name="service-connect-concepts-general"></a>

The Service Connect feature creates a virtual network of related services\. The same service configuration can be used across multiple different namespaces to run independent yet identical sets of applications\. Service Connect defines the proxy container in the Amazon ECS service\. This way, the same task definition can be used to run identical applications in different namespaces with different Service Connect configurations\. Each task that the Amazon ECS service makes runs a proxy container in the task\.

Service Connect is suitable for connections between Amazon ECS services within the same namespace\. For the following applications, you need to use an additional interconnection method to connect to an Amazon ECS service that is configured with Service Connect:
+ Amazon ECS tasks that are configured in other namespaces
+ Amazon ECS tasks that aren’t configured for Service Connect
+ other applications outside of Amazon ECS

These applications can connect through the Service Connect proxy but can’t resolve Service Connect endpoint names\.

For these applications to resolve the IP addresses of ECS tasks, you need to use another interconnection method\. For a list of interconnection methods, see [Choosing an interconnection method](interconnecting-services.md#connection-options)\.

### Service Connect terminology<a name="service-connect-concepts-terms"></a>

The following are terms that are used with Service Connect \.

**port name**  
The Amazon ECS task definition configuration that assigns a name to a particular port mapping\. This configuration is only used by Amazon ECS Service Connect\.

**client alias**  
The Amazon ECS service configuration that assigns the port number that is used in the endpoint\. Additionally, the client alias can assign the DNS name of the endpoint, overriding the discovery name\. If a discovery name isn't provided in the Amazon ECS service, the client alias name overrides the port name as the endpoint name\. For endpoint examples, see the definition of *endpoint*\. Multiple client aliases can be assigned to an Amazon ECS service\. This configuration is only used by Amazon ECS Service Connect\.

**discovery name**  
The optional, intermediate name that you can create for a specified port from the task definition\. This name is used to create a AWS Cloud Map service\. If this name isn't provided, the port name from the task definition is used\. Multiple discovery names can be assigned to a specific port an Amazon ECS service\. This configuration is only used by Amazon ECS Service Connect\.  
AWS Cloud Map service names must be unique within a namespace\. Because of this limitation, you can have only one Service Connect configuration without a discovery name for a particular task definition in each namespace\.

**endpoint**  
The URL to connect to an API or website\. The URL contains the protocol, a DNS name, and the port\. For more information about endpoints in general, see [endpoint](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#endpoint) in the *AWS glossary* in the Amazon Web Services General Reference\.  
Service Connect creates endpoints that connect to Amazon ECS services and configures the tasks in Amazon ECS services to connect to the endpoints\. The URL contains the protocol, a DNS name, and the port\. You select the protocol and port name in the task definition, as the port must match the application that is inside the container image\. In the service, you select each port by name and can assign the DNS name\. If you don't specify a DNS name in the Amazon ECS service configuration, the port name from the task definition is used by default\. For example, a Service Connect endpoint could be `http://blog:80`, `grpc://checkout:8080`, or `http://_db.production.internal:99`\.

**Service Connect service**  
The configuration of a single endpoint in an Amazon ECS service\. This is a part of the Service Connect configuration, consisting of a single row in the **Service Connect and discovery name configuration** in the console, or one object in the `services` list in the JSON configuration of an Amazon ECS service\. This configuration is only used by Amazon ECS Service Connect\.  
For more information, see [ServiceConnectService](http://amazonaws.com/AmazonECS/latest/APIReference/API_ServiceConnectService.html) in the Amazon Elastic Container Service API Reference\.

**namespace**  
The short name or full Amazon Resource Name \(ARN\) of the AWS Cloud Map namespace for use with Service Connect\. The namespace must be in the same AWS Region as the Amazon ECS service and cluster\. The type of namespace in AWS Cloud Map doesn't affect Service Connect\.  
Service Connect uses the AWS Cloud Map namespace as a logical grouping of Amazon ECS tasks that talk to one another\. Each Amazon ECS service can belong to only one namespace\. The services within a namespace can be spread across different Amazon ECS clusters within the same AWS Region in the same AWS account\. Because each cluster can run tasks of every operating system, CPU architecture, VPC, and EC2, Fargate, and External types, you can freely organize your services by any criteria that you choose\.

**client service**  
An Amazon ECS service that runs a network client application\. This service must have a namespace configured\. Each task in the service can discover and connect to all of the endpoints in the namespace through a Service Connect proxy container\.  
If any of your containers in the task need to connect to an endpoint from a service in a namespace, choose a client service\.

**client\-server service**  
An Amazon ECS service that runs a network or web service application\. This service must have a namespace and at least one endpoint configured\. Each task in the service is reachable by using the endpoints\. The Service Connect proxy container listens on the endpoint name and port to direct traffic to the app containers in the task\.  
If any of the containers expose and listen on a port for network traffic, choose a client\-server service\.

### Cluster configuration<a name="service-connect-concepts-cluster-defaults"></a>

You can set a default namespace for Service Connect when you create the cluster or by updating the cluster\. If you specify a namespace name that doesn't exist in the same AWS Region and account, a new HTTP namespace is created\.

Service Connect adds an attachment to Amazon ECS clusters\. The creation of a cluster now waits in the `PROVISIONING` status for the attachment to complete\.

### Service Connect service configuration<a name="service-connect-concepts-config"></a>

Service Connect is designed to require the minimum configuration\. You need to set a name for each port mapping that you would like to use with Service Connect in the task definition\. In the service, you need to turn on Service Connect and select a namespace to make a client service\. To make a client\-server service, you need to add a single Service Connect service configuration that matches the name of one of the port mappings\. Amazon ECS reuses the port number and port name from the task definition to define the Service Connect service and endpoint\. To override those values, you can use the other parameters **Discovery**, **DNS**, and **Port** in the console, or `discoveryName` and `clientAliases`, respectively in the Amazon ECS API\.

The following example shows each kind of Service Connect configuration being used together in the same Amazon ECS service\. Shell comments are provided, however note that the JSON configuration used to Amazon ECS services doesn't support comments\.

```
  {
  ...
  serviceConnectConfiguration: {
   enabled: true,
   namespace: "internal", #config for client services can end here, only these two parameters are required.
   services: [
  { portName: "http" }, #minimal client-server service config can end here. portName must match the "name" parameter of a port mapping in the task definition.
  {
    discoveryName: "http-second" #name the discoveryName to avoid a Task def port name collision with the minimal config in the same Cloud Map namespace
    portName: "http"
  },
  {
    clientAliases: [{dnsName: "db", port: 81}] #use when the port in Task def is not the port that client apps use. Client apps can use http://db:81 to connect
    discoveryName: "http-three"
    portName: "http"
  },
  {
    clientAliases: [{dnsName: "db.app", port: 81}] #use when the port in Task def is not the port that client apps use. duplicates are fine as long as the discoveryName is different.
    discoveryName: "http-four"
    portName: "http",
    ingressPortOverride: 99 #If App should also accept traffic directly on Task def port.
  }
  ]
  }
```

### Deployment order<a name="service-connect-concepts-deploy"></a>

When you use Amazon ECS Service Connect, you configure each Amazon ECS service either to run a server application that receives network requests \(client\-server service\) or to run a client application that makes the requests \(client service\)\.

When you prepare to start using Service Connect, start with a client\-server service\. You can add a Service Connect configuration to a new service or an existing service\. After you edit and update an Amazon ECS service to add a Service Connect configuration, Amazon ECS creates a Service Connect endpoint in the namespace\. Additionally, Amazon ECS creates a new deployment in the service to replace the tasks that are currently running\.

Existing tasks and other applications can continue to connect 

Existing tasks can't resolve and connect to the new endpoint\. Only new Amazon ECS tasks that have a Service Connect configuration in the same namespace and that start running after this deployment can resolve and connect to this endpoint\. For example, an Amazon ECS service that runs a client application must start new tasks after the deployment completes of the server that it connects to\.

This means that the operator of the client application determines when the configuration of their app changes, even though the operator of the server application can change their configuration at any time\. The list of endpoints in the namespace can change every time that any Amazon ECS service in the namespace is deployed\.

### Networking<a name="service-connect-concepts-network"></a>

In the default configuration, you don't need to change your Amazon VPC security groups to use Service Connect\. The Service Connect proxy listens on the `containerPort` from the port mapping in the task definition\.

Even if you set a port number in the Service Connect service configuration, this doesn't change the port for the client\-server service that the Service Connect proxy listens on\. When you set this port number, Amazon ECS changes the port of the endpoint that the client services connect to, on the Service Connect proxy inside those tasks\. The proxy in the client service connects to the proxy in the client\-server service using the `containerPort`\.

If you want to change the port that the Service Connect proxy listens on, change the `ingressPortOverride` in the Service Connect configuration of the client\-server service\. If you change this port number, you must allow inbound traffic on this port in the Amazon VPC security group that is used by traffic to this service\.

Traffic that your applications send to Amazon ECS services configured for Service Connect require that the Amazon VPC and subnets have route table rules and network ACL rules that allow the `containerPort` and `ingressOverridePort` port numbers you are using\.

 You can send traffic between VPCs with Service Connect\. You must consider the same requirements for the route table rules, network ACLs, and security groups as they apply to both VPCs\.

For example, two clusters create tasks in different VPCs\. A service in each cluster is configured to use the same namespace\. The applications in these two services can resolve every endpoint in the namespace without any VPC DNS configuration\. However, the proxies can't connect unless the VPC peering, VPC or subnet route tables, and VPC network ACLs allow the traffic on the `containerPort` and `ingressOverridePort` port numbers you are using

### Service Connect proxy<a name="service-connect-concepts-proxy"></a>

If an Amazon ECS service is created with or updated with Service Connect configuration, Amazon ECS adds a new container to each new task as they are started\. This container isn't present in the task definition and you can't configure it\. Amazon ECS manages the configuration of this container in the Amazon ECS service\. Because of this, you can reuse the same task definitions between multiple Amazon ECS services, namespaces, and you can run tasks without Service Connect also\.

The only configuration for this container in the task definition is the task CPU and memory limits\. The only container configuration for this container in the ECS service is the log configuration inside the Service Connect configuration\.

The task definition must set the task memory limit to use Service Connect\. The additional CPU and memory in the task limits that you don't allocate in the container limits in your other containers are used by the Service Connect proxy container and other containers that don't set container limits\.

We recommend adding 256 CPU units and at least 64 MiB of memory to your task CPU and memory for the Service Connect proxy container\. On AWS Fargate, the lowest amount of memory that you can set is 1024 MiB of memory\. On Amazon EC2, task memory is optional, but it is required for Service Connect\.

If you expect tasks in this service to receive more than 500 requests per second at their peak load, we recommend adding 512 CPU units to your task CPU in this task definition for the Service Connect proxy container\.

If you expect to create more than 100 Service Connect services in the namespace or 2000 tasks in total across all Amazon ECS services within the namespace, we recommend adding 128 MiB of memory to your task memory for the Service Connect proxy container\. You should do this in every task definition that is used by all of the Amazon ECS services in the namespace\.

### Service Connect parameters<a name="service-connect-parameters"></a>

The following parameters have extra fields when using Service Connect\.


****  

| Parameter location | App type | Description | Required? | 
| --- | --- | --- | --- | 
| Task definition | Client | There are no changes available for Service Connect in client task definitions\. | N/A | 
| Task definition | Client\-server | Servers must add name fields to ports in the portMappings of containers\. For more information, see [portMappings](task_definition_parameters.md#ContainerDefinition-portMappings) | Yes | 
| Task definition | Client\-server | Servers can optionally provide an application protocol \(for example, HTTP\) to receive protocol\-specific metrics for their server applications \(for example, HTTP 5xx\)\. | No | 
| Service definition | Client | Client services must add a serviceConnectConfiguration to configure the namespace to join\. This namespace must contain all of the server services that this service needs to discover\. For more information, see [serviceConnectConfiguration](service_definition_parameters.md#Service-serviceConnectConfiguration)\. | Yes | 
| Service definition | Client\-server | Server services must add a serviceConnectConfiguration to configure the DNS names, port numbers, and namespace that the service is available from\. For more information, see [serviceConnectConfiguration](service_definition_parameters.md#Service-serviceConnectConfiguration)\. | Yes | 
| Cluster | Client | Clusters can add a default Service Connect namespace\. New services in the cluster inherit the namespace when Service Connect is configured in a service\. For more information, see [Amazon ECS clusters](clusters.md#clusters-default-service-connect)\. | No | 
| Cluster | Client\-server | There are no changes available for Service Connect in clusters that apply to server services\. Server task definitions and services must set the respective configuration\. | N/A | 

## Service Connect considerations<a name="service-connect-considerations"></a>
+ Windows containers aren't supported with Service Connect\.
+ Container instances must run the Amazon ECS\-optimized Amazon Linux 2 AMI, use the version `2.0.20221115` or later to use Service Connect\. 
+ `External` container instance for Amazon ECS Anywhere aren't supported with Service Connect\.
+ Only services that use rolling deployments are supported with Service Connect\. Services that use the *blue/green* and *external* deployment types aren’t supported\.
+ Tasks that run in Fargate must run the Fargate Linux platform version 1\.4\.0 or higher to use Service Connect\.
+ Task definitions must set the task memory limit to use Service Connect\. For more information, see [Service Connect proxy](#service-connect-concepts-proxy)\.
+ Task definitions that set container memory limits for all containers instead of setting the task memory limit aren't supported\.

  You can set container memory limits on your containers, but you must set the task memory limit to a number greater than the sum of the container memory limits\. The additional CPU and memory in the task limits that aren't allocated in the container limits are used by the Service Connect proxy container and other containers that don't set container limits\. For more information, see [Service Connect proxy](#service-connect-concepts-proxy)\.
+ You can configure Service Connect in a service to use any AWS Cloud Map namespace in the same AWS Region in the same AWS account\. 
+ Each Amazon ECS service can belong to only one namespace\.
+ Only the tasks that Amazon ECS services create are supported\. Standalone tasks can't be configured for Service Connect\.
+ All endpoints must be unique within a namespace\.
+ All discovery names must be unique within a namespace\.
+ Tasks that run in a namespace can only resolve endpoints that already existed before the task started\. New endpoints that are added to the namespace after the task starts won't be added to the task configuration\. For more information, see [Deployment order](#service-connect-concepts-deploy)\.

**Important**  
If the first service in a new or empty namespace is client\-only, the deployment doesn't complete\. You must make a client\-server service as the first service in a new or empty namespace\.

## Service Connect console experience<a name="service-connect-console"></a>

Service Connect management is available only in the new Amazon ECS console\. 

To create a new namespace, either create a new Amazon ECS cluster using the Amazon ECS console and specify a namespace name to create, or use the AWS Cloud Map console\. Amazon ECS Service Connect can use any *instance discovery* type of AWS Cloud Map namespace\. We recommend the *API calls* type to make the minimum amount of additional resources\. To create a new Amazon ECS cluster and namespace in the Amazon ECS console, see [Creating a cluster for the Fargate launch type using the new console](create-cluster-console-v2.md)\.

Every AWS Cloud Map namespace in this AWS account in the selected AWS Region is displayed in the **Namespaces** in the Amazon ECS console\.

To delete a namespace, use the AWS Cloud Map console\. A namespace must be empty before it can be deleted\.

To create a new Amazon ECS task definition, or register a new revision to an existing task definition and use Service Connect, see [Creating a task definition using the new console](create-task-definition.md)\.

To create a new Amazon ECS service that uses Service Connect, see [Creating a service using the new console](create-service-console-v2.md)\.

## Service Connect pricing<a name="service-connect-pricing"></a>

Amazon ECS Service Connect pricing depends on whether you use AWS Fargate or Amazon EC2 infrastructure to host your containerized workloads\. When using Amazon ECS on AWS Outposts, the pricing follows the same model that's used when you use Amazon EC2 directly\. For more information, see [Amazon ECS Pricing](https://aws.amazon.com/ecs/pricing)\.