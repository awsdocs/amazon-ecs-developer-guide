# Getting started with AWS App Mesh and Amazon ECS<a name="appmesh-getting-started"></a>

AWS App Mesh is a service mesh based on the [Envoy](https://www.envoyproxy.io/) proxy that helps you monitor and control services\. App Mesh standardizes how your services communicate, giving you end\-to\-end visibility into and helping to ensure high\-availability for your applications\. App Mesh gives you consistent visibility and network traffic controls for every service in an application\. For more information, see the [AWS App Mesh User Guide](https://docs.aws.amazon.com/app-mesh/latest/userguide/)\.

This topic helps you use AWS App Mesh with an actual service that is running on Amazon ECS\. This tutorial covers basic features of several App Mesh resource types\. To learn more about features of resources that aren't used when completing this tutorial, see the topics for [virtual nodes](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_nodes.html), [virtual services](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_services.html), [virtual routers](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_routers.html), [routes](https://docs.aws.amazon.com/app-mesh/latest/userguide/routes.html), and the [Envoy proxy](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html)\.

## Scenario<a name="scenario"></a>

To illustrate how to use App Mesh with Amazon ECS, assume that you have an application with the following characteristics:
+ Includes two services named `serviceA` and `serviceB`\. 
+ Both services are registered to a namespace named `apps.local`\.
+ `ServiceA` communicates with `serviceB` over HTTP/2, port 80\.
+  You have already deployed version 2 of `serviceB` and registered it with the name `serviceBv2` in the `apps.local` namespace\.

You have the following requirements:
+ You want to send 75 percent of the traffic from `serviceA` to `serviceB` and 25 percent of the traffic to `serviceBv2` to ensure that `serviceBv2` is bug free before you send 100 percent of the traffic from `serviceA` to it\. 
+ You want to be able to easily adjust the traffic weighting so that 100 percent of the traffic goes to `serviceBv2` once it is proven to be reliable\. Once all traffic is being sent to `serviceBv2`, you want to deprecate `serviceB`\.
+ You do not want to have to change any existing application code or service discovery registration for your actual services to meet the previous requirements\. 

To meet your requirements, you have decided to create an App Mesh service mesh with virtual services, virtual nodes, a virtual router, and a route\. After implementing your mesh, you update the task definitions for your services to use the Envoy proxy\. Once updated, your services communicate with each other through the Envoy proxy rather than directly with each other\.

## Prerequisites<a name="prerequisites"></a>

App Mesh supports Linux services that are registered with DNS, AWS Cloud Map, or both\. To use this getting started guide, we recommend that you have three existing services that are registered with DNS\. You can create a service mesh and its resources even if the services don't exist, but you cannot use the mesh until you have deployed actual services\.

For more information about service discovery on Amazon ECS, see [Service Discovery](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-discovery.html)\. To create an Amazon ECS service with service discovery, see [Tutorial: Creating a Service Using Service Discovery](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service-discovery.html)\.

The remaining steps assume that the actual services are named `serviceA`, `serviceB`, and `serviceBv2` and that all services are discoverable through a namespace named `apps.local`\. 

## Step 1: Create a mesh and virtual service<a name="create-mesh-and-virtual-service"></a>

A service mesh is a logical boundary for network traffic between the services that reside within it\. For more information, see [Service Meshes](https://docs.aws.amazon.com/app-mesh/latest/userguide/meshes.html) in the *AWS App Mesh User Guide*\. A virtual service is an abstraction of an actual service\. For more information, see [Virtual Services](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_services.html) in the *AWS App Mesh User Guide*\. 

Create the following resources:
+ A mesh named `apps`, since all of the services in the scenario are registered to the `apps.local` namespace\.
+ A virtual service named `serviceb.apps.local`, since the virtual service represents a service that is discoverable with that name, and you don't want to change your code to reference another name\. A virtual service named `servicea.apps.local` is added in a later step\.

You can use the AWS Management Console or the AWS CLI version 1\.18\.116 or higher or 2\.0\.38 or higher to complete the following steps\. If using the AWS CLI, use the `aws --version` command to check your installed AWS CLI version\. If you don't have version 1\.18\.116 or higher or 2\.0\.38 or higher installed, then you must [install or update the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)\. Select the tab for the tool that you want to use\.

------
#### [ AWS Management Console ]

1. Open the App Mesh console first\-run wizard at [https://console\.aws\.amazon\.com/appmesh/get\-started](https://console.aws.amazon.com/appmesh/get-started)\.

1. For **Mesh name**, enter **apps**\.

1. For **Virtual service name**, enter **serviceb\.apps\.local**\.

1. To continue, choose **Next**\.

------
#### [ AWS CLI ]

1. Create a mesh with the `[create\-mesh](https://docs.aws.amazon.com/cli/latest/reference/appmesh/create-mesh.html)` command\.

   ```
   aws appmesh create-mesh --mesh-name apps
   ```

1. Create a virtual service with the `[create\-virtual\-service](https://docs.aws.amazon.com/cli/latest/reference/appmesh/create-virtual-service.html)` command\.

   ```
   aws appmesh create-virtual-service --mesh-name apps --virtual-service-name serviceb.apps.local --spec {}
   ```

------

## Step 2: Create a virtual node<a name="create-virtual-node"></a>

A virtual node acts as a logical pointer to an actual service\. For more information, see [Virtual Nodes](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_nodes.html) *in the AWS App Mesh User Guide*\. 

Create a virtual node named `serviceB`, since one of the virtual nodes represents the actual service named `serviceB`\. The actual service that the virtual node represents is discoverable through `DNS` with a hostname of `serviceb.apps.local`\. Alternately, you can discover actual services using AWS Cloud Map\. The virtual node will listen for traffic using the HTTP/2 protocol on port 80\. Other protocols are also supported, as are health checks\. You will create virtual nodes for `serviceA` and `serviceBv2` in a later step\.

------
#### [ AWS Management Console ]

1. For **Virtual node name**, enter **serviceB**\. 

1. For **Service discovery method**, choose **DNS** and enter **serviceb\.apps\.local** for **DNS hostname**\.

1. Under **Listener configuration**, choose **http2** for **Protocol** and enter **80** for **Port**\.

1. To continue, choose **Next**\.

------
#### [ AWS CLI ]

1. Create a file named `create-virtual-node-serviceb.json` with the following contents:

   ```
   {
       "meshName": "apps",
       "spec": {
           "listeners": [
               {
                   "portMapping": {
                       "port": 80,
                       "protocol": "http2"
                   }
               }
           ],
           "serviceDiscovery": {
               "dns": {
                   "hostname": "serviceB.apps.local"
               }
           }
       },
       "virtualNodeName": "serviceB"
   }
   ```

1. Create the virtual node with the [create\-virtual\-node](https://docs.aws.amazon.com/cli/latest/reference/appmesh/create-virtual-node.html) command using the JSON file as input\.

   ```
   aws appmesh create-virtual-node --cli-input-json file://create-virtual-node-serviceb.json
   ```

------

## Step 3: Create a virtual router and route<a name="create-virtual-router-and-route"></a>

Virtual routers route traffic for one or more virtual services within your mesh\. For more information, see [Virtual Routers](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_routers.html) and [Routes](https://docs.aws.amazon.com/app-mesh/latest/userguide/routes.html) in the *AWS App Mesh User Guide*\.

Create the following resources:
+ A virtual router named `serviceB`, since the `serviceB.apps.local` virtual service does not initiate outbound communication with any other service\. Remember that the virtual service that you created previously is an abstraction of your actual `serviceb.apps.local` service\. The virtual service sends traffic to the virtual router\. The virtual router will listen for traffic using the HTTP/2 protocol on port 80\. Other protocols are also supported\. 
+ A route named `serviceB`\. It will route 100 percent of its traffic to the `serviceB` virtual node\. You will change the weight in a later step once you have added the `serviceBv2` virtual node\. Though not covered in this guide, you can add additional filter criteria for the route and add a retry policy to cause the Envoy proxy to make multiple attempts to send traffic to a virtual node when it experiences a communication problem\.

------
#### [ AWS Management Console ]

1. For **Virtual router name,** enter **serviceB**\.

1. Under **Listener configuration**, choose **http2** for **Protocol** and specify **80** for **Port**\.

1. For **Route name**, enter **serviceB**\. 

1. For **Route type**, choose **http2**\.

1. For **Virtual node name** under **Route configuration**, select `serviceB` and enter **100** for **Weight**\.

1. To continue, choose **Next**\.

------
#### [ AWS CLI ]

1. Create a virtual router\.

   1. Create a file named `create-virtual-router.json` with the following contents:

      ```
      {
          "meshName": "apps",
          "spec": {
              "listeners": [
                  {
                      "portMapping": {
                          "port": 80,
                          "protocol": "http2"
                      }
                  }
              ]
          },
          "virtualRouterName": "serviceB"
      }
      ```

   1. Create the virtual router with the [create\-virtual\-router](https://docs.aws.amazon.com/cli/latest/reference/appmesh/create-virtual-router.html) command using the JSON file as input\.

      ```
      aws appmesh create-virtual-router --cli-input-json file://create-virtual-router.json
      ```

1. Create a route\.

   1. Create a file named `create-route.json` with the following contents:

      ```
      {
          "meshName" : "apps",
          "routeName" : "serviceB",
          "spec" : {
              "httpRoute" : {
                  "action" : {
                      "weightedTargets" : [
                          {
                              "virtualNode" : "serviceB",
                              "weight" : 100
                          }
                      ]
                  },
                  "match" : {
                      "prefix" : "/"
                  }
              }
          },
          "virtualRouterName" : "serviceB"
      }
      ```

   1. Create the route with the [create\-route](https://docs.aws.amazon.com/cli/latest/reference/appmesh/create-route.html) command using the JSON file as input\.

      ```
      aws appmesh create-route --cli-input-json file://create-route.json
      ```

------

## Step 4: Review and create<a name="review-create"></a>

Review the settings against the previous instructions\.

------
#### [ AWS Management Console ]

Choose **Edit** if you need to make changes in any section\. Once you are satisfied with the settings, choose **Create mesh**\.

The **Status** screen shows you all of the mesh resources that were created\. You can see the created resources in the console by selecting **View mesh**\.

------
#### [ AWS CLI ]

Review the settings of the mesh you created with the [describe\-mesh](https://docs.aws.amazon.com/cli/latest/reference/appmesh/describe-mesh.html) command\.

```
aws appmesh describe-mesh --mesh-name apps
```

Review the settings of the virtual service that you created with the [describe\-virtual\-service](https://docs.aws.amazon.com/cli/latest/reference/appmesh/describe-virtual-service.html) command\.

```
aws appmesh describe-virtual-service --mesh-name apps --virtual-service-name serviceb.apps.local
```

Review the settings of the virtual node that you created with the [describe\-virtual\-node](https://docs.aws.amazon.com/cli/latest/reference/appmesh/describe-virtual-node.html) command\.

```
aws appmesh describe-virtual-node --mesh-name apps --virtual-node-name serviceB
```

Review the settings of the virtual router that you created with the [describe\-virtual\-router](https://docs.aws.amazon.com/cli/latest/reference/appmesh/describe-virtual-router.html) command\.

```
aws appmesh describe-virtual-router --mesh-name apps --virtual-router-name serviceB
```

Review the settings of the route that you created with the [describe\-route](https://docs.aws.amazon.com/cli/latest/reference/appmesh/describe-route.html) command\.

```
aws appmesh describe-route --mesh-name apps \
    --virtual-router-name serviceB  --route-name serviceB
```

------

## Step 5: Create additional resources<a name="create-additional-resources"></a>

To complete the scenario, you need to:
+ Create one virtual node named `serviceBv2` and another named `serviceA`\. Both virtual nodes listen for requests over HTTP/2 port 80\. For the `serviceA` virtual node, configure a backend of `serviceb.apps.local`, since all outbound traffic from the `serviceA` virtual node is sent to the virtual service named `serviceb.apps.local`\. Though not covered in this guide, you can also specify a file path to write access logs to for a virtual node\.
+ Create one additional virtual service named `servicea.apps.local`, which will send all traffic directly to the `serviceA` virtual node\.
+ Update the `serviceB` route that you created in a previous step to send 75 percent of its traffic to the `serviceB` virtual node and 25 percent of its traffic to the `serviceBv2` virtual node\. Over time, you can continue to modify the weights until `serviceBv2` receives 100 percent of the traffic\. Once all traffic is sent to `serviceBv2`, you can deprecate the `serviceB` virtual node and actual service\. As you change weights, your code does not require any modification, because the `serviceb.apps.local` virtual and actual service names don't change\. Recall that the `serviceb.apps.local` virtual service sends traffic to the virtual router, which routes the traffic to the virtual nodes\. The service discovery names for the virtual nodes can be changed at any time\.

------
#### [ AWS Management Console ]

1. In the left navigation pane, select **Meshes**\.

1. Select the `apps` mesh that you created in a previous step\.

1. In the left navigation pane, select **Virtual nodes**\.

1. Choose **Create virtual node**\.

1. For **Virtual node name**, enter **serviceBv2**, for **Service discovery method**, choose **DNS**, and for **DNS hostname**, enter **servicebv2\.apps\.local**\.

1. For **Listener configuration**, select **http2** for **Protocol** and enter **80** for **Port**\.

1. Choose **Create virtual node**\.

1. Choose **Create virtual node** again\. Enter **serviceA** for the **Virtual node name**\. For **Service discovery method**, choose **DNS**, and for **DNS hostname**, enter **servicea\.apps\.local**\.

1. For **Enter a virtual service name** under **New backend**, enter **servicea\.apps\.local**\.

1. Under **Listener configuration**, choose **http2** for **Protocol**, enter **80** for **Port**, and then choose **Create virtual node**\.

1. In the left navigation pane, select** Virtual routers** and then select the `serviceB` virtual router from the list\.

1. Under **Routes**, select the route named `ServiceB` that you created in a previous step, and choose **Edit**\.

1. Under **Targets**, **Virtual node name**, change the value of **Weight** for `serviceB` to **75**\.

1. Choose **Add target**, choose `serviceBv2` from the drop\-down list, and set the value of **Weight** to **25**\.

1. Choose **Save**\.

1. In the left navigation pane, select** Virtual services** and then choose **Create virtual service**\.

1. Enter **servicea\.apps\.local** for **Virtual service name**, select **Virtual node** for **Provider**, select `serviceA` for **Virtual node**, and then choose **Create virtual service\.**

------
#### [ AWS CLI ]

1. Create the `serviceBv2` virtual node\.

   1. Create a file named `create-virtual-node-servicebv2.json` with the following contents:

      ```
      {
          "meshName": "apps",
          "spec": {
              "listeners": [
                  {
                      "portMapping": {
                          "port": 80,
                          "protocol": "http2"
                      }
                  }
              ],
              "serviceDiscovery": {
                  "dns": {
                      "hostname": "serviceBv2.apps.local"
                  }
              }
          },
          "virtualNodeName": "serviceBv2"
      }
      ```

   1. Create the virtual node\.

      ```
      aws appmesh create-virtual-node --cli-input-json file://create-virtual-node-servicebv2.json
      ```

1. Create the `serviceA` virtual node\.

   1. Create a file named `create-virtual-node-servicea.json` with the following contents:

      ```
      {
         "meshName" : "apps",
         "spec" : {
            "backends" : [
               {
                  "virtualService" : {
                     "virtualServiceName" : "serviceb.apps.local"
                  }
               }
            ],
            "listeners" : [
               {
                  "portMapping" : {
                     "port" : 80,
                     "protocol" : "http2"
                  }
               }
            ],
            "serviceDiscovery" : {
               "dns" : {
                  "hostname" : "servicea.apps.local"
               }
            }
         },
         "virtualNodeName" : "serviceA"
      }
      ```

   1. Create the virtual node\.

      ```
      aws appmesh create-virtual-node --cli-input-json file://create-virtual-node-servicea.json
      ```

1. Update the `serviceb.apps.local` virtual service that you created in a previous step to send its traffic to the `serviceB` virtual router\. When the virtual service was originally created, it did not send traffic anywhere, since the `serviceB` virtual router had not been created yet\.

   1. Create a file named `update-virtual-service.json` with the following contents:

      ```
      {
         "meshName" : "apps",
         "spec" : {
            "provider" : {
               "virtualRouter" : {
                  "virtualRouterName" : "serviceB"
               }
            }
         },
         "virtualServiceName" : "serviceb.apps.local"
      }
      ```

   1. Update the virtual service with the [update\-virtual\-service](https://docs.aws.amazon.com/cli/latest/reference/appmesh/update-virtual-service.html) command\.

      ```
      aws appmesh update-virtual-service --cli-input-json file://update-virtual-service.json
      ```

1. Update the `serviceB` route that you created in a previous step\.

   1. Create a file named `update-route.json` with the following contents:

      ```
      {
         "meshName" : "apps",
         "routeName" : "serviceB",
         "spec" : {
            "http2Route" : {
               "action" : {
                  "weightedTargets" : [
                     {
                        "virtualNode" : "serviceB",
                        "weight" : 75
                     },
                     {
                        "virtualNode" : "serviceBv2",
                        "weight" : 25
                     }
                  ]
               },
               "match" : {
                  "prefix" : "/"
               }
            }
         },
         "virtualRouterName" : "serviceB"
      }
      ```

   1. Update the route with the [update\-route](https://docs.aws.amazon.com/cli/latest/reference/appmesh/update-route.html) command\.

      ```
      aws appmesh update-route --cli-input-json file://update-route.json
      ```

1. Create the `serviceA` virtual service\.

   1. Create a file named `create-virtual-servicea.json` with the following contents:

      ```
      {
         "meshName" : "apps",
         "spec" : {
            "provider" : {
               "virtualNode" : {
                  "virtualNodeName" : "serviceA"
               }
            }
         },
         "virtualServiceName" : "servicea.apps.local"
      }
      ```

   1. Create the virtual service\.

      ```
      aws appmesh create-virtual-service --cli-input-json file://create-virtual-servicea.json
      ```

------

**Mesh summary**  
Before you created the service mesh, you had three actual services named `servicea.apps.local`, `serviceb.apps.local`, and `servicebv2.apps.local`\. In addition to the actual services, you now have a service mesh that contains the following resources that represent the actual services:
+ Two virtual services\. The proxy sends all traffic from the `servicea.apps.local` virtual service to the `serviceb.apps.local` virtual service through a virtual router\. 
+ Three virtual nodes named `serviceA`, `serviceB`, and `serviceBv2`\. The Envoy proxy uses the service discovery information configured for the virtual nodes to look up the IP addresses of the actual services\. 
+ One virtual router with one route that instructs the Envoy proxy to route 75 percent of inbound traffic to the `serviceB` virtual node and 25 percent of the traffic to the `serviceBv2` virtual node\. 

## Step 6: Update services<a name="update-services"></a>

After creating your mesh, you need to complete the following tasks:
+ Authorize the Envoy proxy that you deploy with each Amazon ECS task to read the configuration of one or more virtual nodes\. For more information about how to authorize the proxy, see [Proxy authorization](https://docs.aws.amazon.com/app-mesh/latest/userguide/proxy-authorization.html)\.
+ Update each of your existing Amazon ECS task definitions to use the Envoy proxy\. 

**Credentials**  
The Envoy container requires AWS Identity and Access Management credentials for signing requests that are sent to the App Mesh service\. For Amazon ECS tasks deployed with the Amazon EC2 launch type, the credentials can come from the [instance role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html) or from a [task IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)\. Amazon ECS tasks deployed with the Fargate launch type don't have access to the Amazon EC2 metadata server that supplies instance IAM profile credentials\. To supply the credentials, you must attach an IAM task role to any tasks deployed with the Fargate launch type\. If a task is deployed with the Amazon EC2 launch type and access is blocked to the Amazon EC2 metadata server, as described in the *Important* annotation in [IAM Role for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html), then a task IAM role must also be attached to the task\. The role that you assign to the instance or task must have an IAM policy attached to it as described in [Proxy authorization](https://docs.aws.amazon.com/app-mesh/latest/userguide/proxy-authorization.html)\.

**Update task definitions**  
You can update your task definitions by using the AWS Management Console or by modifying the JSON file for a task definition\. The following steps only show updating the `taskB` task for the scenario\. You also need to update the `taskBv2` and `taskA` tasks by changing the values appropriately\. Select the method you prefer to use to update the task definition\.

------
#### [ AWS Management Console ]

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region that contains your task definition\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, select the box to the left of the task definition to revise\. From the pre\-requisites and previous steps, you might have task definitions named `taskA`, `taskB`, and `taskBv2`\. Select `taskB` and choose **Create new revision**\.

1. On the **Create new revision of Task Definition** page, make the following changes to enable App Mesh integration\.

   1. For **Service Integration**, to configure the parameters for App Mesh integration choose **Enable App Mesh integration** and then do the following:

      1. For **Application container name**, choose the container name to use for the App Mesh application\. This container must already be defined within the task definition\.

      1. For **Envoy image**, complete the following task and enter the value that is returned\.
         + All [supported](https://docs.aws.amazon.com/general/latest/gr/appmesh.html) Regions other than `me-south-1` and `ap-east-1`\. You can replace *region\-code* with any Region other than `me-south-1` and `ap-east-1`\. 

           ```
           840364872350.dkr.ecr.region-code.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
           ```
         + `me-south-1` Region:

           ```
           772975370895.dkr.ecr.me-south-1.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
           ```
         + `ap-east-1` Region:

           ```
           856666278305.dkr.ecr.ap-east-1.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
           ```

      1. For **Mesh name**, choose the App Mesh service mesh to use\. In this topic, the name of the mesh that was created is `apps.`

      1. For **Virtual node name**, choose the App Mesh virtual node to use\. For example, for the `taskB` task, you would choose the `serviceB` virtual node that you created in a previous step\.

      1. The value for **Virtual node port** is pre\-populated with the listener port that you specified when you created the virtual node\.

      1. Choose **Apply**, and then choose **Confirm**\. A new Envoy proxy container is created and added to the task definition, and the settings to support the container are also created\. The Envoy proxy container then pre\-populates the App Mesh **Proxy Configuration** settings for the next step\.

   1. For **Proxy Configuration**, verify all of the pre\-populated values\.

   1. For **Network Mode**, ensure that `awsvpc` is selected\. To learn more about the `awsvpc` network mode, see [Task Networking with the `awsvpc` Network Mode](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking.html)\.

1. Choose **Create**\.

1. Update your service with the updated task definition\. For more information, see [Updating a service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/update-service.html)\.

------
#### [ JSON ]

**Proxy configuration**  
To configure your Amazon ECS service to use App Mesh, your service's task definition must have the following proxy configuration section\. Set the proxy configuration `type` to `APPMESH` and the `containerName` to `envoy`\. Set the following property values accordingly\.

`IgnoredUID`  
The Envoy proxy doesn't route traffic from processes that use this user ID\. You can choose any user ID that you want for this property value, but this ID must be the same as the `user` ID for the Envoy container in your task definition\. This matching allows Envoy to ignore its own traffic without using the proxy\. Our examples use `1337` for historical purposes\.

`ProxyIngressPort`  
This is the ingress port for the Envoy proxy container\. Set this value to `15000`\.

`ProxyEgressPort`  
This is the egress port for the Envoy proxy container\. Set this value to `15001`\.

`AppPorts`  
Specify any ingress ports that your application containers listen on\. In this example, the application container listens on port `9080`\. The port that you specify must match the port configured on the virtual node listener\.

`EgressIgnoredIPs`  
Envoy doesn't proxy traffic to these IP addresses\. Set this value to `169.254.170.2,169.254.169.254`, which ignores the Amazon EC2 metadata server and the Amazon ECS task metadata endpoint\. The metadata endpoint provides IAM roles for tasks credentials\. You can add additional addresses\.

`EgressIgnoredPorts`  
You can add a comma separated list of ports\. Envoy doesn't proxy traffic to these ports\. Even if you list no ports, port 22 is ignored\.

```
"proxyConfiguration": {
	"type": "APPMESH",
	"containerName": "envoy",
	"properties": [{
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
		},
		{
			"name": "EgressIgnoredPorts",
			"value": "22"
		}
	]
}
```

**Application container Envoy dependency**  
The application containers in your task definitions must wait for the Envoy proxy to bootstrap and start before they can start\. To ensure that this happens, you set a `dependsOn` section in each application container definition to wait for the Envoy container to report as `HEALTHY`\. The following code shows an application container definition example with this dependency\. All of the properties in the following example are required\. Some of the property values are also required, but some are *replaceable*\.

```
{
	"name": "appName",
	"image": "appImage",
	"portMappings": [{
		"containerPort": 9080,
		"hostPort": 9080,
		"protocol": "tcp"
	}],
	"essential": true,
	"dependsOn": [{
		"containerName": "envoy",
		"condition": "HEALTHY"
	}]
}
```

**Envoy container definition**

Your Amazon ECS task definitions must contain an App Mesh Envoy container image\.
+ All [supported](https://docs.aws.amazon.com/general/latest/gr/appmesh.html) Regions other than `me-south-1` and `ap-east-1`\. You can replace *region\-code* with any Region other than `me-south-1` and `ap-east-1`\. 

  ```
  840364872350.dkr.ecr.region-code.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
  ```
+ `me-south-1` Region:

  ```
  772975370895.dkr.ecr.me-south-1.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
  ```
+ `ap-east-1` Region:

  ```
  856666278305.dkr.ecr.ap-east-1.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod
  ```

You must use the App Mesh Envoy container image until the Envoy project team merges changes that support App Mesh\. For additional details, see the [GitHub roadmap issue](https://github.com/aws/aws-app-mesh-roadmap/issues/10)\.

All of the properties in the following example are required\. Some of the property values are also required, but some are *replaceable*\. The Envoy container definition must be marked as `essential`\. The virtual node name for the Amazon ECS service must be set to the value of the `APPMESH_VIRTUAL_NODE_NAME` property\. The value for the `user` setting must match the `IgnoredUID` value from the task definition proxy configuration\. In this example, we use `1337`\. The health check shown here waits for the Envoy container to bootstrap properly before reporting to Amazon ECS that the Envoy container is healthy and ready for the application containers to start\.

The following code shows an Envoy container definition example\.

```
{
	"name": "envoy",
	"image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
	"essential": true,
	"environment": [{
		"name": "APPMESH_VIRTUAL_NODE_NAME",
		"value": "mesh/apps/virtualNode/serviceB"
	}],
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

**Example task definitions**  
The following example Amazon ECS task definitions show how to merge the examples from above into a task definition for `taskB`\. Examples are provided for creating tasks for both Amazon ECS launch types with or without using AWS X\-Ray\. Change the *replaceable* values, as appropriate, to create task definitions for the tasks named `taskBv2` and `taskA` from the scenario\. Substitute your mesh name and virtual node name for the `APPMESH_VIRTUAL_NODE_NAME` value and a list of ports that your application listens on for the proxy configuration `AppPorts` value\. All of the properties in the following examples are required\. Some of the property values are also required, but some are *replaceable*\.

If you're running an Amazon ECS task as described in the Credentials section, then you need to add an existing [task IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html), to the examples\.

**Example JSON for Amazon ECS task definition \- Fargate launch type**  

```
{
   
   "family" : "taskB",
   "memory" : "1024",
   "cpu" : "0.5 vCPU",
   "proxyConfiguration" : {
      "containerName" : "envoy",
      "properties" : [
         {
            "name" : "ProxyIngressPort",
            "value" : "15000"
         },
         {
            "name" : "AppPorts",
            "value" : "9080"
         },
         {
            "name" : "EgressIgnoredIPs",
            "value" : "169.254.170.2,169.254.169.254"
         },
         {
            "name": "EgressIgnoredPorts",
            "value": "22"
         },
         {
            "name" : "IgnoredUID",
            "value" : "1337"
         },
         {
            "name" : "ProxyEgressPort",
            "value" : "15001"
         }
      ],
      "type" : "APPMESH"
   },
   "containerDefinitions" : [
      {
         "name" : "appName",
         "image" : "appImage",
         "portMappings" : [
            {
               "containerPort" : 9080,
               "protocol" : "tcp"
            }
         ],
         "essential" : true,
         "dependsOn" : [
            {
               "containerName" : "envoy",
               "condition" : "HEALTHY"
            }
         ]
      },
      {         
         "name" : "envoy",
         "image" : "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
         "essential" : true,
         "environment" : [
            {
               "name" : "APPMESH_VIRTUAL_NODE_NAME",
               "value" : "mesh/apps/virtualNode/serviceB"
            }
         ],
         "healthCheck" : {
            "command" : [
               "CMD-SHELL",
               "curl -s http://localhost:9901/server_info | grep state | grep -q LIVE"
            ],
            "interval" : 5,
            "retries" : 3,
            "startPeriod" : 10,
            "timeout" : 2
         },
         "memory" : "500",
         "user" : "1337"
      }
   ],
   "requiresCompatibilities" : [ "FARGATE" ],
   "taskRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskRole",
   "executionRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
   "networkMode" : "awsvpc"
}
```

**Example JSON for Amazon ECS task definition with AWS X\-Ray \- Fargate launch type**  
X\-Ray allows you to collect data about requests that an application serves and provides tools that you can use to visualize traffic flow\. Using the X\-Ray driver for Envoy enables Envoy to report tracing information to X\-Ray\. You can enable X\-Ray tracing using the [Envoy configuration](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html)\. Based on the configuration, Envoy sends tracing data to the X\-Ray daemon running as a [sidecar](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-ecs.html) container and the daemon forwards the traces to the X\-Ray service\. Once the traces are published to X\-Ray, you can use the X\-Ray console to visualize the service call graph and request trace details\. The following JSON represents a task definition to enable X\-Ray integration\.  

```
{
   
   
   "family" : "taskB",
   "memory" : "1024",
   "cpu" : "0.5 vCPU",
   "proxyConfiguration" : {
      "containerName" : "envoy",
      "properties" : [
         {
            "name" : "ProxyIngressPort",
            "value" : "15000"
         },
         {
            "name" : "AppPorts",
            "value" : "9080"
         },
         {
            "name" : "EgressIgnoredIPs",
            "value" : "169.254.170.2,169.254.169.254"
         },
         {
            "name": "EgressIgnoredPorts",
            "value": "22"
         },
         {
            "name" : "IgnoredUID",
            "value" : "1337"
         },
         {
            "name" : "ProxyEgressPort",
            "value" : "15001"
         }
      ],
      "type" : "APPMESH"
   },
   "containerDefinitions" : [
      {
         "name" : "appName",
         "image" : "appImage",
         "portMappings" : [
            {
               "containerPort" : 9080,
               "protocol" : "tcp"
            }
         ],
         "essential" : true,
         "dependsOn" : [
            {
               "containerName" : "envoy",
               "condition" : "HEALTHY"
            }
         ]
      },
      {
         
         "name" : "envoy",
         "image" : "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
         "essential" : true,
         "environment" : [
            {
               "name" : "APPMESH_VIRTUAL_NODE_NAME",
               "value" : "mesh/apps/virtualNode/serviceB"
            },
            {
               "name": "ENABLE_ENVOY_XRAY_TRACING",
               "value": "1"
            }
         ],
         "healthCheck" : {
            "command" : [
               "CMD-SHELL",
               "curl -s http://localhost:9901/server_info | grep state | grep -q LIVE"
            ],
            "interval" : 5,
            "retries" : 3,
            "startPeriod" : 10,
            "timeout" : 2
         },
         "memory" : "500",
         "user" : "1337"
      },
      {
         "name" : "xray-daemon",
         "image" : "amazon/aws-xray-daemon",
         "user" : "1337",
         "essential" : true,
         "cpu" : "32",
         "memoryReservation" : "256",
         "portMappings" : [
            {
               "containerPort" : 2000,
               "protocol" : "udp"
            }
         ]
      }
   ],
   "requiresCompatibilities" : [ "FARGATE" ],
   "taskRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskRole",
   "executionRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
   "networkMode" : "awsvpc"
}
```

**Example JSON for Amazon ECS task definition \- EC2 launch type**  

```
{
  "family": "taskB",
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
      },
      {
        "name": "EgressIgnoredPorts",
        "value": "22"
      }
    ]
  },
  "containerDefinitions": [
    {
      "name": "appName",
      "image": "appImage",
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
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/apps/virtualNode/serviceB"
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
  "requiresCompatibilities" : [ "EC2" ],
  "taskRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskRole",
  "executionRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc"
}
```

**Example JSON for Amazon ECS task definition with AWS X\-Ray \- EC2 launch type**  
X\-Ray allows you to collect data about requests that an application serves and provides tools that you can use to visualize traffic flow\. Using the X\-Ray driver for Envoy enables Envoy to report tracing information to X\-Ray\. You can enable X\-Ray tracing using the [Envoy configuration](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html)\. Based on the configuration, Envoy sends tracing data to the X\-Ray daemon running as a [sidecar](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-ecs.html) container and the daemon forwards the traces to the X\-Ray service\. Once the traces are published to X\-Ray, you can use the X\-Ray console to visualize the service call graph and request trace details\. The following JSON represents a task definition to enable X\-Ray integration\.  

```
{
  "family": "taskB",
  "memory": "256",
   "cpu" : "1024",
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
      },
      {
        "name": "EgressIgnoredPorts",
        "value": "22"
      }
    ]
  },
  "containerDefinitions": [
    {
      "name": "appName",
      "image": "appImage",
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
      "image": "840364872350.dkr.ecr.us-west-2.amazonaws.com/aws-appmesh-envoy:v1.15.0.0-prod",
      "essential": true,
      "environment": [
        {
          "name": "APPMESH_VIRTUAL_NODE_NAME",
          "value": "mesh/apps/virtualNode/serviceB"
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
  "requiresCompatibilities" : [ "EC2" ],
  "taskRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskRole",
  "executionRoleArn" : "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc"
}
```

------