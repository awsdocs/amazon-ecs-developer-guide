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

A virtual node acts as a logical pointer to an Amazon ECS service\. For more information, see [Virtual Nodes](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_nodes.html) in the *AWS App Mesh User Guide*\.

When you create a virtual node, you must specify a service discovery method for your task group\. Any inbound traffic that your virtual node expects should be specified as a *listener*\. Any outbound traffic that your virtual node expects to reach should be specified as a *backend*\.

You must create virtual nodes for each microservice in your application\.

**To create a virtual node in the AWS Management Console\.**

1. Choose the mesh that you created in the previous steps\.

1. Choose **Virtual nodes** in the left navigation\.

1. Choose **Create virtual node**\. 

1. For **Virtual node name**, enter a name for your virtual node\. 

1. For **Service discovery method**, choose one of the following options:
   + **DNS** – Specify the DNS\-registered hostname of the actual service that the virtual node represents\. For additional information about using DNS as a service discovery method, see [Virtual Nodes](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_nodes.html)\. 
   + **AWS Cloud Map** – Specify the service name and namespace\. Optionally, you can also specify attributes that App Mesh can query AWS Cloud Map for\. Only instances that match all of the specified key/value pairs will be returned\. To use AWS Cloud Map, your account must have the `AWSServiceRoleForAppMesh` [service\-linked role](using-service-linked-roles.md)\. 

1. To specify any backends \(for egress traffic\) for your virtual node, or to configure inbound and outbound access logging information, choose **Additional configuration** 

   1. To specify a backend, choose **Add backend** and enter a virtual service name or full Amazon Resource Name \(ARN\) for the virtual service that your virtual node communicates with\. Repeat this step until all of your virtual node backends are accounted for\. 

   1. To configure logging, enter the HTTP access logs path that you want Envoy to use\. We recommend the `/dev/stdout` path so that you can use Docker log drivers to export your Envoy logs to a service such as Amazon CloudWatch Logs\. 
**Note**  
Logs must still be ingested by an agent in your application and sent to a destination\. This file path only instructs Envoy where to send the logs\. 

1. Specify a **Port** and **Protocol** for the **Listener**\. 

1. If you want to configure health checks for your listener, ensure that **Health check enabled** is selected and then complete the following substeps\. If not, clear this check box\. 

   1. For **Health check protocol**, choose to use an HTTP or TCP health check\. 

   1. For **Health check port**, specify the port that the health check should run on\. 

   1. For **Healthy threshold**, specify the number of consecutive successful health checks that must occur before declaring the listener healthy\. 

   1. For **Health check interval**, specify the time period in milliseconds between each health check execution\. 

   1. For **Path**, specify the destination path for the health check request\. This is required only if the specified protocol is HTTP\. If the protocol is TCP, this parameter is ignored\. 

   1. For **Timeout period**, specify the amount of time to wait when receiving a response from the health check, in milliseconds\. 

   1. For **Unhealthy threshold**, specify the number of consecutive failed health checks that must occur before declaring the listener unhealthy\. 

1. Choose **Create virtual node** to finish\. 

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

 A route is associated with a virtual router, and it' used to match requests for a virtual router and distribute traffic accordingly to its associated virtual nodes\. For more information, see [Routes](https://docs.aws.amazon.com//app-mesh/latest/userguide/routes.html) in the *AWS App Mesh User Guide*\.

Create routes for each microservice in your application\.

**Creating a route in the AWS Management Console\.**

1. Choose **Virtual routers** in the left navigation\.

1. Choose the virtual router that you want to associate a new route with\. 

1. In the **Routes** table, choose **Create route**\.

1. For **Route name**, specify the name to use for your route\.

1. For **Route type**, choose the protocol for your route\.

1. \(Optional\) For **Route priority**, specify a priority from 0\-1000 to use for your route\. Routes are matched based on the specified value, where 0 is the highest priority\.

1. For **Virtual node name**, choose the virtual node that this route will serve traffic to\. 

1. For **Weight**, choose a relative weight for the route\. Select **Add target** to add additional virtual nodes\. The total weight for all targets combined must be less than or equal to 100\.

1. \(Optional\) To use HTTP path and header\-based routing, choose **Additional configuration**\. 

1. \(Optional\) To use HTTP path\-based routing, specify the **Prefix** that the route should match\. For additional information about path\-based routing, see [Path\-based Routing](https://docs.aws.amazon.com//app-mesh/latest/userguide/route-path.html)\. 

1. \(Optional\) Select a **Method** to use header\-based routing for your route\. For additional information about HTTP header\-based routing, see [HTTP Headers](https://docs.aws.amazon.com//app-mesh/latest/userguide/route-http-headers.html)\. 

1. \(Optional\) Select a **Scheme** to use header\-based routing for your route\. 

1. \(Optional\) Select **Add header**\. Enter the **Header name** that you want to route based on, select a **Match type**, and enter a **Match value**\. Selecting **Invert** will match the opposite\. 

1. \(Optional\) Select **Add header** to add up to ten headers\. 

1. Choose **Create route** to finish\.

1. Repeat this procedure as necessary to create routes for each remaining microservice in your application\.

## Step 5: Create Your Virtual Services<a name="mesh-gs-ecs-create-virtual-services"></a>

A virtual service is an abstraction of a real service that is provided by a virtual node directly or indirectly by means of a virtual router\. Dependent services call your virtual service by its `virtualServiceName`, and those requests are routed to the virtual node or virtual router that is specified as the provider for the virtual service\. For more information, see [Virtual Services](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_services.html) in the *AWS App Mesh User Guide*\.

Create virtual services for each microservice in your application\.

**Creating a virtual service in the AWS Management Console\.**

1. Choose **Virtual services** in the left navigation\. 

1. Choose **Create virtual service** 

1. For **Virtual service name**, choose a name for your virtual service\. We recommend that you use the service discovery name of the real service that you're targeting \(such as `service-a.default.svc.cluster.local`\)\. The name that you specify must resolve to a non\-loopback IP address\.

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

Your Amazon ECS services' task definitions must contain the App Mesh custom Envoy container image\. You can replace the *Region* with any Region that App Mesh is supported in\. For a list of supported regions, see [AWS Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#appmesh_region)\.

```
840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.11.2.0-prod
```

The Envoy container definition must be marked as `essential`\. The virtual node name for the Amazon ECS service should be set to the `APPMESH_VIRTUAL_NODE_NAME`, and the `user` ID value should match the `IgnoredUID` value from the task definition proxy configuration \(in this example, we use `1337`\)\. The health check shown here waits for the Envoy container to bootstrap properly before reporting to Amazon ECS that it is healthy and ready for the application containers to start\.

The following code block shows an Envoy container definition example\.

```
    {
      "name": "envoy",
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.11.2.0-prod",
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

### Credentials<a name="credentials"></a>

The Envoy container requires AWS Identity and Access Management credentials for signing requests that are sent to the App Mesh service\. For Amazon ECS tasks deployed with the Amazon EC2 launch type, the credentials can come from the [instance IAM role](instance_IAM_role.md) or from a [task IAM role](task-iam-roles.md)\. Amazon ECS tasks deployed with the Fargate launch type do not have access to the Amazon EC2 metadata server that supplies instance IAM profile credentials\. To supply the credentials, you must attach an IAM task role to any tasks deployed with the Fargate launch type\. The role doesn't need to have a policy attached to it, but for a task to work properly with App Mesh, the role must be attached to each task deployed with the Fargate launch type\. If a task is deployed with the Amazon EC2 launch type and access is blocked to the Amazon EC2 metadata server, as described in the *Important* annotation in [IAM Roles for Tasks](task-iam-roles.md), then a task IAM role must also be attached to the task\. 

### Update an Existing Task Definition<a name="mesh-gs-ecs-update-task-def"></a>

The Amazon ECS console assists in the process of updating your existing task definitions to add App Mesh integration\.

**Update a task definition to add App Mesh integration**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region that contains your task definition\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, select the box to the left of the task definition to revise and choose **Create new revision**\.

1. On the **Create new revision of Task Definition** page, make the following changes to enable App Mesh integration\.

   1. For **Service Integration**, to configure the parameters for App Mesh integration choose **Enable App Mesh integration** and then do the following:

      1. For **Application container name**, choose the container name to use for the App Mesh application\. This container must already be defined within the task definition\.

      1. For **Envoy image**, use the auto\-populated Envoy container image which is 840364872350\.dkr\.ecr\.*us\-west\-2*\.amazonaws\.com/aws\-appmesh\-envoy:v1\.11\.2\.0\-prod\.

      1. For **Mesh name**, choose the App Mesh service mesh to use\. This must already be created in order for it to show up\. For more information, see [Service Meshes](https://docs.aws.amazon.com//app-mesh/latest/userguide/meshes.html) in the *AWS App Mesh User Guide*\.

      1. For **Virtual node name**, choose the App Mesh virtual node to use\. This must already be created in order for it to show up\. For more information, see [Virtual Nodes](https://docs.aws.amazon.com//app-mesh/latest/userguide/virtual_nodes.html) in the *AWS App Mesh User Guide*\.

      1. For **Virtual node port**, this will be pre\-populated with the listener port set on the virtual node\.

      1. Choose **Apply**, **Confirm**\. This will create a new Envoy proxy container to the task definition, as well as the settings to support it\. It will then pre\-populate the App Mesh proxy configuration settings for the next step\.

   1. For **Proxy Configuration**, verify all of the pre\-populated values\. For more information on these fields, see [Proxy Configuration](#mesh-gs-ecs-proxyconfig)\.

1. Verify the information and choose **Create**\.

1. If your task definition is used in a service, update your service with the updated task definition\. For more information, see [Updating a Service](update-service.md)\.

### Example Task Definitions<a name="mesh-gs-ecs-task-def"></a>

The following example Amazon ECS task definitions show, in context, the snippets that you can merge with your existing task groups\. Substitute your mesh name and virtual node name for the `APPMESH_VIRTUAL_NODE_NAME` value and a list of ports that your application listens on for the proxy configuration `AppPorts` value\.

If you're running an Amazon ECS task as described in [Credentials](#credentials), you need an existing [task IAM role](task-iam-roles.md)\. You should also add this line of code to the example task definitions that follow: `"taskRoleArn": "arn:aws:iam::123456789012:role/ecsTaskRole"` 

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
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.11.2.0-prod",
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

**Example JSON for Amazon ECS task definition with AWS X\-Ray**  
X\-Ray allows you to collect data about requests that an application serves and provides tools that you can use to visualize traffic flow\. Using the X\-Ray driver for Envoy enables Envoy to report tracing information to X\-Ray\. You can enable X\-Ray tracing using the [Envoy configuration](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html)\. Based on the configuration, Envoy sends tracing data to the X\-Ray daemon running as a [sidecar](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-ecs.html) container and the daemon forwards the traces to the X\-Ray service\. Once the traces are published to X\-Ray, you can use the X\-Ray console to visualize the service call graph and request trace details\. The following JSON represents a task definition to enable X\-Ray integration\.  

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
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.11.2.0-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/meshName/virtualNode/virtualNodeName"
        },
        {
         "name": "ENABLE_ENVOY_XRAY_TRACING",
         "value": "1"
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
    },
    {
      "name": "xray-daemon",
      "image": "amazon/aws-xray-daemon",
      "user": "1337",
      "essential": true,
      "cpu": 32,
      "memoryReservation": 256,
      "portMappings": [
        {
          "containerPort": 2000,
          "protocol": "udp"
        }
      ]
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc"
}
```