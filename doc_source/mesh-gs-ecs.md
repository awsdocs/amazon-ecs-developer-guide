# Getting Started with AWS App Mesh and Amazon ECS<a name="mesh-gs-ecs"></a>

AWS App Mesh is a service mesh based on the [Envoy](https://www.envoyproxy.io/) proxy that makes it easy to monitor and control microservices\. App Mesh standardizes how your microservices communicate, giving you end\-to\-end visibility and helping to ensure high\-availability for your applications\.

App Mesh gives you consistent visibility and network traffic controls for every microservice in an application\. For more information, see the [AWS App Mesh User Guide](https://docs.aws.amazon.com//app-mesh/latest/userguide/)\.

This topic helps you to use AWS App Mesh with an existing microservice application running on Amazon ECS\.

## Prerequisites<a name="mesh-gs-ecs-prerequisites"></a>

App Mesh supports microservice applications that use service discovery naming for their components\. To use this getting started guide, you must have a microservice application running on Amazon ECS that already has service discovery configured\.

For more information about service discovery on Amazon ECS, see [Service Discovery](service-discovery.md)\.

## Step 1: Create Your Service Mesh<a name="mesh-gs-ecs-create-mesh"></a>

A service mesh is a logical boundary for network traffic between the services that reside within it\. For more information, see [Service Meshes](https://docs.aws.amazon.com//app-mesh/latest/userguide/meshes.html) in the *AWS App Mesh User Guide*\.

After you create your service mesh, you can create virtual services, virtual nodes, virtual routers, and routes to distribute traffic between the applications in your mesh\.

**To create a new service mesh with the AWS Management Console**

1. Open the App Mesh console at [https://console\.aws\.amazon\.com/appmesh/](https://console.aws.amazon.com/appmesh/)\.

1. Choose **Create mesh**\.

1. For **Mesh name**, specify a name for your service mesh\.

1. Choose **Create mesh** to finish\.

## Step 2: Create Your Virtual Nodes<a name="mesh-gs-ecs-create-virtual-nodes"></a>

A virtual node acts as a logical pointer to a particular task group, such as a Kubernetes deployment\. For more information, see [Virtual Nodes](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_nodes.html) in the *AWS App Mesh User Guide*\.

When you create a virtual node, you must specify the DNS service discovery hostname for your task group\. Any inbound traffic that your virtual node expects should be specified as a *listener*\. Any outbound traffic that your virtual node expects to reach should be specified as a *backend*\.

You must create virtual nodes for each microservice in your application\.

**To create a virtual node in the AWS Management Console\.**

1. Choose the mesh that you created in the previous steps\.

1. Choose **Virtual nodes** in the left navigation\.

1. Choose **Create virtual node**\.

1. For **Virtual node name**, choose a name for your virtual node\.

1. For **Service discovery method**, choose **DNS** for services that use DNS service discovery and then specify the hostname for **DNS hostname**\. Otherwise, choose **None** if your virtual node doesn't expect any ingress traffic\.

1. To specify any backends \(for egress traffic\) for your virtual node, or to configure inbound and outbound access logging information, choose **Additional configuration**\.

   1. To specify a backend, choose **Add backend** and enter a virtual service name or full Amazon Resource Name \(ARN\) for the virtual service that your virtual node communicates with\. Repeat this step until all of your virtual node backends are accounted for\.

   1. To configure logging, enter the HTTP access logs path that you want Envoy to use\. We recommend the `/dev/stdout` path so that you can use Docker log drivers to export your Envoy logs to a service such as Amazon CloudWatch Logs\.
**Note**  
Logs must still be ingested by an agent in your application and sent to a destination\. This file path only instructs Envoy where to send the logs\.

1. If your virtual node expects ingress traffic, specify a **Port** and **Protocol** for that **Listener**\.

1. If you want to configure health checks for your listener, ensure that **Health check enabled** is selected and then complete the following substeps\. If not, clear this check box\.

   1. For **Health check protocol**, choose to use an HTTP or TCP health check\.

   1. For **Health check port**, specify the port that the health check should run on\.

   1. For **Healthy threshold**, specify the number of consecutive successful health checks that must occur before declaring the listener healthy\.

   1. For **Health check interval**, specify the time period in milliseconds between each health check execution\.

   1. For **Path**, specify the destination path for the health check request\. This is required only if the specified protocol is HTTP\. If the protocol is TCP, this parameter is ignored\.

   1. For **Timeout period**, specify the amount of time to wait when receiving a response from the health check, in milliseconds\.

   1. For **Unhealthy threshold**, specify the number of consecutive failed health checks that must occur before declaring the listener unhealthy\.

1. Chose **Create virtual node** to finish\.

1. Repeat this procedure as necessary to create virtual nodes for each remaining microservice in your application\.

## Step 3: Create Your Virtual Routers<a name="mesh-gs-ecs-create-virtual-routers"></a>

Virtual routers handle traffic for one or more virtual services within your mesh\. After you create a virtual router, you can create and associate routes for your virtual router that direct incoming requests to different virtual nodes\. For more information, see [Virtual Routers](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_routers.html) in the *AWS App Mesh User Guide*\.

Create virtual routers for each microservice in your application\.

**Creating a virtual router in the AWS Management Console\.**

1. Choose **Virtual routers** in the left navigation\.

1. Choose **Create virtual router**\.

1. For **Virtual router name**, specify a name for your virtual router\. Up to 255 letters, numbers, hyphens, and underscores are allowed\.

1. For **Listener**, specify a **Port** and **Protocol** for your virtual router\.

1. Choose **Create virtual router** to finish\.

1. Repeat this procedure as necessary to create virtual routers for each remaining microservice in your application\.

## Step 4: Create Your Routes<a name="mesh-gs-ecs-create-routes"></a>

A route is associated with a virtual router, and it's used to match requests for a virtual router and distribute traffic accordingly to its associated virtual nodes\. For more information, see [Routes](https://docs.aws.amazon.com//app-mesh/latest/userguide/routes.html) in the *AWS App Mesh User Guide*\.

Create routes for each microservice in your application\.

**Creating a route in the AWS Management Console\.**

1. Choose **Virtual routers** in the left navigation\.

1. Choose the router that you want to associate a new route with\.

1. In the **Routes** table, choose **Create route**\.

1. For **Route name**, specify the name to use for your route\.

1. For **Route type**, choose the protocol for your route\.

1. For **Virtual node name**, choose the virtual node that this route will serve traffic to\.

1. For **Weight**, choose a relative weight for the route\. The total weight for all routes must be less than 100\.

1. To use HTTP path\-based routing, choose **Additional configuration** and then specify the path that the route should match\. For example, if your virtual service name is `my-service.local` and you want the route to match requests to `my-service.local/metrics`, your prefix should be `/metrics`\.

1. Choose **Create route** to finish\.

1. Repeat this procedure as necessary to create routes for each remaining microservice in your application\.

## Step 5: Create Your Virtual Services<a name="mesh-gs-ecs-create-virtual-services"></a>

A virtual service is an abstraction of a real service that is provided by a virtual node directly or indirectly by means of a virtual router\. Dependent services call your virtual service by its `virtualServiceName`, and those requests are routed to the virtual node or virtual router that is specified as the provider for the virtual service\. For more information, see [Virtual Services](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_services.html) in the *AWS App Mesh User Guide*\.

Create virtual services for each microservice in your application\.

**Creating a virtual service in the AWS Management Console\.**

1. Choose **Virtual services** in the left navigation\.

1. Choose **Create virtual service**\.

1. For **Virtual service name**, choose a name for your virtual service\. We recommend that you use the service discovery name of the real service that you're targeting \(such as `my-service.default.svc.cluster.local`\)\.

1. For **Provider**, choose the provider type for your virtual service:
   + If you want the virtual service to spread traffic across multiple virtual nodes, select **Virtual router** and then choose the virtual router to use from the drop\-down menu\.
   + If you want the virtual service to reach a virtual node directly, without a virtual router, select **Virtual node** and then choose the virtual node to use from the drop\-down menu\.
   + If you don't want the virtual service to route traffic at this time \(for example, if your virtual nodes or virtual router doesn't exist yet\), choose **None**\. You can update the provider for this virtual service later\.

1. Choose **Create virtual service** to finish\.

1. Repeat this procedure as necessary to create virtual services for each remaining microservice in your application\.

## Update Your Microservice Task Definitions<a name="mesh-gs-ecs-update-microservices"></a>

After you create your service mesh, virtual nodes, virtual routers, routes, and virtual services, you must update the Amazon ECS task definitions for your microservices to be compatible with App Mesh\. Complete the steps in the following sections to update your services' task definitions to work with App Mesh\. When you are finished, update your Amazon ECS services to start using App Mesh with your Amazon ECS application\.

### Proxy Configuration<a name="mesh-gs-ecs-proxyconfig"></a>

To configure your Amazon ECS service to use App Mesh, your service's task definition must have the following proxy configuration section\. Set the proxy configuration `type` to `APPMESH` and the `containerName` to `envoy`\. Set the following property values accordingly\.

`IgnoredUID`  
Envoy doesn't proxy traffic from processes that use this user ID\. You can choose any user ID that you want for this \(our examples use `1337` for historical purposes\), but this ID must be the same as the `user` ID for the Envoy container in your task definition\. This matching allows Envoy to ignore its own traffic without using the proxy\.

`ProxyIngressPort`  
This is the ingress port for the Envoy proxy container\. Set this value to `15000`\.

`ProxyEgressPort`  
This is the egress port for the Envoy proxy container\. Set this value to `15001`\.

`AppPorts`  
Specify any ingress ports that your application containers listen on\. In this example, the application container listens on port `9080`\.

`EgressIgnoredIPs`  
Envoy doesn't proxy traffic to these IP addresses\. Set this value to `169.254.170.2,169.254.169.254`, which ignores the Amazon EC2 metadata server and the Amazon ECS task metadata endpoint \(which provides IAM roles for tasks credentials\)\.

```
  "proxyConfiguration": {
    "type": "APPMESH",
    "containerName": "envoy",
    "properties": [
      {
        "name": "IgnoredUID",
        "value": "1337"
      },
      {
        "name": "ProxyIngressPort",
        "value": "15000"
      },
      {
        "name": "ProxyEgressPort",
        "value": "15001"
      },
      {
        "name": "AppPorts",
        "value": "9080"
      },
      {
        "name": "EgressIgnoredIPs",
        "value": "169.254.170.2,169.254.169.254"
      }
    ]
  }
```

### Application Container Envoy Dependency<a name="mesh-gs-ecs-envoy-dep"></a>

The application containers in your task definitions must wait for the Envoy proxy to bootstrap and start before they can start\. To ensure that this happens, you set a `dependsOn` section in each application container definition to wait for the Envoy container to report as `HEALTHY`\. The following code block shows an application container definition example with this dependency\.

```
    {
      "name": "app",
      "image": "application_image",
      "portMappings": [
        {
          "containerPort": 9080,
          "hostPort": 9080,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "dependsOn": [
        {
          "containerName": "envoy",
          "condition": "HEALTHY"
        }
      ]
    }
```

### Envoy Container Definition<a name="mesh-gs-ecs-envoy"></a>

Your Amazon ECS services' task definitions must contain the App Mesh custom Envoy container image\.

```
111345817488.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.9.1.0-prod
```

The Envoy container definition must be marked as `essential`\. The virtual node name for the Amazon ECS service should be set to the `APPMESH_VIRTUAL_NODE_NAME`, and the `user` ID value should match the `IgnoredUID` value from the task definition proxy configuration \(in this example, we use `1337`\)\. The health check shown here waits for the Envoy container to bootstrap properly before reporting to Amazon ECS that it is healthy and ready for the application containers to start\.

The following code block shows an Envoy container definition example\.

```
    {
      "name": "envoy",
      "image": "111345817488.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.9.1.0-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/meshName/virtualNode/virtualNodeName"
        }
      ],
      "healthCheck": {
        "command": [
          "CMD-SHELL",
          "curl -s http://localhost:9901/server_info | grep state | grep -q LIVE"
        ],
        "startPeriod": 10,
        "interval": 5,
        "timeout": 2,
        "retries": 3
      },
      "user": "1337"
    }
```

### Example Task Definition<a name="mesh-gs-ecs-task-def"></a>

The following example Amazon ECS task definition shows in context the snippets that you can merge with your existing task groups\. Substitute your mesh name and virtual node name for the `APPMESH_VIRTUAL_NODE_NAME` value and a list of ports that your application listens on for the proxy configuration `AppPorts` value\.

**Example JSON for Amazon ECS task definition**  

```
{
  "family": "appmesh-gateway",
  "memory": "256",
  "proxyConfiguration": {
    "type": "APPMESH",
    "containerName": "envoy",
    "properties": [
      {
        "name": "IgnoredUID",
        "value": "1337"
      },
      {
        "name": "ProxyIngressPort",
        "value": "15000"
      },
      {
        "name": "ProxyEgressPort",
        "value": "15001"
      },
      {
        "name": "AppPorts",
        "value": "9080"
      },
      {
        "name": "EgressIgnoredIPs",
        "value": "169.254.170.2,169.254.169.254"
      }
    ]
  },
  "containerDefinitions": [
    {
      "name": "app",
      "image": "application_image",
      "portMappings": [
        {
          "containerPort": 9080,
          "hostPort": 9080,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "dependsOn": [
        {
          "containerName": "envoy",
          "condition": "HEALTHY"
        }
      ]
    },
    {
      "name": "envoy",
      "image": "111345817488.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.9.1.0-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/meshName/virtualNode/virtualNodeName"
        }
      ],
      "healthCheck": {
        "command": [
          "CMD-SHELL",
          "curl -s http://localhost:9901/server_info | grep state | grep -q LIVE"
        ],
        "startPeriod": 10,
        "interval": 5,
        "timeout": 2,
        "retries": 3
      },
      "user": "1337"
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc"
}
```