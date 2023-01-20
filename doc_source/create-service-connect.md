# Tutorial: Using Service Connect in Fargate with the AWS CLI<a name="create-service-connect"></a>

The following tutorial shows how to create an Amazon ECS service containing a Fargate task that uses Service Connect with the AWS CLI\.

Amazon ECS supports the Service Connect feature in the AWS Regions listed in [Regions with Service Connect](service-connect.md#service-connect-regions)\.

## Prerequisites<a name="create-service-connect-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The latest version of the AWS CLI is installed and configured\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\.
+ The steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ You have a VPC, subnet, route table, and security group created to use\. For more information, see [Create a virtual private cloud](get-set-up-for-amazon-ecs.md#create-a-vpc)\.
+ You have a task execution role with the name `ecsTaskExecutionRole` and the `AmazonECSTaskExecutionRolePolicy` managed policy is attached to the role\. This role allows Fargate to write the NGINX application logs and Service Connect proxy logs to Amazon CloudWatch Logs\. For more information, see [Creating the task execution IAM role](task_execution_IAM_role.md#create-task-execution-role)\.

## Step 1: Create the Amazon ECS cluster<a name="create-service-connect-cluster"></a>

Use the following steps to create your Amazon ECS cluster and namespace\.

**To create the Amazon ECS cluster and AWS Cloud Map namespace**

1. Create an Amazon ECS cluster named `tutorial` to use\. The parameter `--service-connect-defaults` sets the default namespace of the cluster\. In the example output, a AWS Cloud Map namespace of the name `service-connect` doesn't exist in this account and AWS Region, so the namespace is created by Amazon ECS\. The namespace is made in AWS Cloud Map in the account, and is visible with all of the other namespaces, so use a name that indicates the purpose\.

   ```
   aws ecs create-cluster --cluster-name tutorial --service-connect-defaults namespace=service-connect
   ```

   Output:

   ```
   {
       "cluster": {
           "clusterArn": "arn:aws:ecs:us-west-2:123456789012:cluster/tutorial",
           "clusterName": "tutorial",
           "serviceConnectDefaults": {
               "namespace": "arn:aws:servicediscovery:us-west-2:123456789012:namespace/ns-EXAMPLE"
           },
           "status": "PROVISIONING",
           "registeredContainerInstancesCount": 0,
           "runningTasksCount": 0,
           "pendingTasksCount": 0,
           "activeServicesCount": 0,
           "statistics": [],
           "tags": [],
           "settings": [
               {
                   "name": "containerInsights",
                   "value": "disabled"
               }
           ],
           "capacityProviders": [],
           "defaultCapacityProviderStrategy": [],
           "attachments": [
               {
                   "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
                   "type": "sc",
                   "status": "ATTACHING",
                   "details": []
               }
           ],
           "attachmentsStatus": "UPDATE_IN_PROGRESS"
       }
   }
   }
   ```

1. Verify that the cluster is created:

   ```
   aws ecs describe-clusters --clusters tutorial
   ```

   Output:

   ```
   {
       "clusters": [
           {
               "clusterArn": "arn:aws:ecs:us-west-2:123456789012:cluster/tutorial",
               "clusterName": "tutorial",
               "serviceConnectDefaults": {
                   "namespace": "arn:aws:servicediscovery:us-west-2:123456789012:namespace/ns-EXAMPLE"
               },
               "status": "ACTIVE",
               "registeredContainerInstancesCount": 0,
               "runningTasksCount": 0,
               "pendingTasksCount": 0,
               "activeServicesCount": 0,
               "statistics": [],
               "tags": [],
               "settings": [],
               "capacityProviders": [],
               "defaultCapacityProviderStrategy": []
           }
       ],
       "failures": []
   }
   ```

1. \(Optional\) Verify that the namespace is created in AWS Cloud Map\. You can use the AWS Management Console or the normal AWS CLI configuration as this is created in AWS Cloud Map\.

   For example, use the AWS CLI:

   ```
   aws servicediscovery --region us-west-2 get-namespace --id ns-EXAMPLE
   ```

   Output:

   ```
   {
       "Namespace": {
           "Id": "ns-EXAMPLE",
           "Arn": "arn:aws:servicediscovery:us-west-2:123456789012:namespace/ns-EXAMPLE",
           "Name": "service-connect",
           "Type": "HTTP",
           "Properties": {
               "DnsProperties": {
                   "SOA": {}
               },
               "HttpProperties": {
                   "HttpName": "service-connect"
               }
           },
           "CreateDate": 1661749852.422,
           "CreatorRequestId": "service-connect"
       }
   }
   ```

## Step 2: Create the Amazon ECS service for the server<a name="create-service-connect-nginx-server"></a>

The Service Connect feature is intended for interconnecting multiple applications on Amazon ECS\. At least one of those applications needs to provide a web service to connect to\. In this step, you create:
+ The task definition that uses the unmodified official NGINX container image and includes Service Connect configuration\.
+ The Amazon ECS service definition that configures Service Connect to provide service discovery and service mesh proxying for traffic to this service\. The configuration reuses the default namespace from the cluster configuration to reduce the amount of service configuration that you make for each service\.
+ The Amazon ECS service\. It runs one task using the task definition, and inserts an additional container for the Service Connect proxy\. The proxy listens on the port from the container port mapping of the task definition\. In a client application running in Amazon ECS, the proxy in the client task listens for outbound connections to the task definition port name, service discovery name or service client alias name, and the port number from the client alias\.

**To create the web service with Amazon ECS Service Connect**

1. Register a task definition that's compatible with Fargate and uses the `awsvpc` network mode\. Follow these steps:

   1. Create a file that's named `service-connect-nginx.json` with the contents of the following task definition\.

      This task definition configures Service Connect by adding `name` and `appProtocol` parameters to the port mapping\. The port name makes this port more identifiable in the service configuration when multiple ports are used\. The port name is also used by default as the discoverable name for use by other applications in the namespace\.

      This task definition uses the `ECSSCTaskRole` role for both the permissions of the Amazon ECS Fargate agent in the task execution role, and the AWS API permissions for the Service Connect proxy container in the task role\.
**Important**  
This task definition uses a `logConfiguration` to send the nginx output from `stdout` and `stderr` to Amazon CloudWatch Logs\. This task execution role doesn't have the extra permissions required to make the CloudWatch Logs log group\. Create the log group in CloudWatch Logs using the AWS Management Console or AWS CLI\. If you don't want to send the nginx logs to CloudWatch Logs you can remove the `logConfiguration`\.

      ```
      {
          "family": "service-connect-nginx",
          "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
          "networkMode": "awsvpc",
          "containerDefinitions": [
              {
              "name": "webserver",
              "image": "public.ecr.aws/docker/library/nginx:latest",
              "cpu": 100,
              "portMappings": [
                  {
                      "name": "nginx",
                      "containerPort": 80,
                      "protocol": "tcp", 
                      "appProtocol": "http"
                  }
              ],
              "essential": true,
              "logConfiguration": {
                  "logDriver": "awslogs",
                  "options": {
                      "awslogs-group": "/ecs/service-connect-nginx",
                      "awslogs-region": "us-west-2", 
                      "awslogs-stream-prefix": "nginx"
                  }
              }
              }
          ],
          "cpu": "256",
          "memory": "512"
      }
      ```

   1. Register the task definition using the `service-connect-nginx.json` file:

      ```
      aws ecs register-task-definition --cli-input-json file://service-connect-nginx.json
      ```

1. Create an ECS service by following these steps:

   1. Create a file that's named `service-connect-nginx-service.json` with the contents of the Amazon ECS service that you're creating\. This example uses the task definition that was created in the previous step\. An `awsvpcConfiguration` is required because the example task definition uses the `awsvpc` network mode\.

      When you create the ECS service, specify the Fargate launch type, and the `LATEST` platform version that supports Service Connect\. The `securityGroups` and `subnets` must belong to a VPC that has the requirements for using Amazon ECS\. You can obtain the security group and subnet IDs from the Amazon VPC Console\. 

      This service configures Service Connect by adding the `serviceConnectConfiguration` parameter\. The namespace is not required because the cluster has a default namespace configured\. Client applications running in ECS in the namespace connect to this service by using the `portName` and the port in the `clientAliases`\. For example, this service is reachable using `http://nginx:80/`, as nginx provides a welcome page in the root location `/`\. External applications that are not running in Amazon ECS or are not in the same namespace can reach this application through the Service Connect proxy by using the IP address of the task and the port number from the task definition\.

      This service uses a `logConfiguration` to send the service connect proxy output from `stdout` and `stderr` to Amazon CloudWatch Logs\. This task execution role doesn't have the extra permissions required to make the CloudWatch Logs log group\. Create the log group in CloudWatch Logs using the AWS Management Console or AWS CLI\. We recommend that you create this log group and store the proxy logs in CloudWatch Logs\. If you don't want to send the proxy logs to CloudWatch Logs you can remove the `logConfiguration`\.

      ```
      {
          "cluster": "tutorial",
          "deploymentConfiguration": {
              "maximumPercent": 200,
              "minimumHealthyPercent": 0
          },
          "deploymentController": {
              "type": "ECS"
          },
          "desiredCount": 1,
          "enableECSManagedTags": true,
          "enableExecuteCommand": true,
          "launchType": "FARGATE",
          "networkConfiguration": {
              "awsvpcConfiguration": {
                  "assignPublicIp": "ENABLED",
                  "securityGroups": [
                      "sg-EXAMPLE"
                  ],
                  "subnets": [
                      "subnet-EXAMPLE",
                      "subnet-EXAMPLE",
                      "subnet-EXAMPLE"
                  ]
                 }
          },
          "platformVersion": "LATEST",
          "propagateTags": "SERVICE",
          "serviceName": "service-connect-nginx-service",
          "serviceConnectConfiguration": {
              "enabled": true,
              "services": [
                  {
                      "portName": "nginx",
                      "clientAliases": [
                          {
                              "port": 80
                          }
                      ]
                  }
              ],
              "logConfiguration": {
                  "logDriver": "awslogs",
                  "options": {
                      "awslogs-group": "/ecs/service-connect-proxy",
                      "awslogs-region": "us-west-2",
                      "awslogs-stream-prefix": "service-connect-proxy"
                  }
              }
          },
          "taskDefinition": "service-connect-nginx"
      }
      ```

   1. Create an ECS service using the `service-connect-nginx-service.json` file:

      ```
      aws ecs create-service --cluster tutorial --cli-input-json file://service-connect-nginx-service.json
      ```

      Output:

      ```
      {
          "service": {
              "serviceArn": "arn:aws:ecs:us-west-2:123456789012:service/tutorial/service-connect-nginx-service",
              "serviceName": "service-connect-nginx-service",
              "clusterArn": "arn:aws:ecs:us-west-2:123456789012:cluster/tutorial",
              "loadBalancers": [],
              "serviceRegistries": [],
              "status": "ACTIVE",
              "desiredCount": 1,
              "runningCount": 0,
              "pendingCount": 0,
              "launchType": "FARGATE",
              "platformVersion": "LATEST",
              "platformFamily": "Linux",
              "taskDefinition": "arn:aws:ecs:us-west-2:123456789012:task-definition/service-connect-nginx:1",
              "deploymentConfiguration": {
                  "deploymentCircuitBreaker": {
                      "enable": false,
                      "rollback": false
                  },
                  "maximumPercent": 200,
                  "minimumHealthyPercent": 0
              },
              "deployments": [
                  {
                      "id": "ecs-svc/3763308422771520962",
                      "status": "PRIMARY",
                      "taskDefinition": "arn:aws:ecs:us-west-2:123456789012:task-definition/service-connect-nginx:1",
                      "desiredCount": 1,
                      "pendingCount": 0,
                      "runningCount": 0,
                      "failedTasks": 0,
                      "createdAt": 1661210032.602,
                      "updatedAt": 1661210032.602,
                      "launchType": "FARGATE",
                      "platformVersion": "1.4.0",
                      "platformFamily": "Linux",
                      "networkConfiguration": {
                          "awsvpcConfiguration": {
                              "assignPublicIp": "ENABLED",
                              "securityGroups": [
                                  "sg-EXAMPLE"
                              ],
                              "subnets": [
                                  "subnet-EXAMPLEf",
                                  "subnet-EXAMPLE",
                                  "subnet-EXAMPLE"
                              ]
                          }
                      },
                      "rolloutState": "IN_PROGRESS",
                      "rolloutStateReason": "ECS deployment ecs-svc/3763308422771520962 in progress.",
                      "failedLaunchTaskCount": 0,
                      "replacedTaskCount": 0,
                      "serviceConnectConfiguration": {
                          "enabled": true,
                          "namespace": "service-connect",
                          "services": [
                              {
                                  "portName": "nginx",
                                  "clientAliases": [
                                      {
                                          "port": 80
                                      }
                                  ]
                              }
                          ],
                          "logConfiguration": {
                              "logDriver": "awslogs",
                              "options": {
                                  "awslogs-group": "/ecs/service-connect-proxy",
                                  "awslogs-region": "us-west-2",
                                  "awslogs-stream-prefix": "service-connect-proxy"
                              },
                              "secretOptions": []
                          }
                      },
                      "serviceConnectResources": [
                          {
                              "discoveryName": "nginx",
                              "discoveryArn": "arn:aws:servicediscovery:us-west-2:123456789012:service/srv-EXAMPLE"
                          }
                      ]
                  }
              ],
              "roleArn": "arn:aws:iam::123456789012:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS",
              "version": 0,
              "events": [],
              "createdAt": 1661210032.602,
              "placementConstraints": [],
              "placementStrategy": [],
              "networkConfiguration": {
                  "awsvpcConfiguration": {
                      "assignPublicIp": "ENABLED",
                      "securityGroups": [
                          "sg-EXAMPLE"
                      ],
                      "subnets": [
                          "subnet-EXAMPLE",
                          "subnet-EXAMPLE",
                          "subnet-EXAMPLE"
                      ]
                  }
              },
              "schedulingStrategy": "REPLICA",
              "enableECSManagedTags": true,
              "propagateTags": "SERVICE",
              "enableExecuteCommand": true
          }
      }
      ```

      The `serviceConnectConfiguration` that you provided appears inside the first *deployment* of the output\. As you make changes to the ECS service in ways that need to make changes to tasks, a new deployment is created by Amazon ECS\.

## Step 3: Verify that you can connect<a name="create-service-connect-verify"></a>

To verify that Service Connect is configured and working, follow these steps to connect to the web service from an external application\. Then, see the additional metrics in CloudWatch that are created by the Service Connect proxy\.

**To connect to the web service from an external application**
+ Connect to the task IP address and container port using the task IP address

  Use the AWS CLI to get the task ID, using the `aws ecs list-tasks --cluster tutorial`\.

  If your subnets and security group permit traffic from the public internet on the port from the task definition, you can connect to the public IP from your computer\. The public IP isn't available from `describe\-tasks` however, so the steps involve going to the Amazon EC2 AWS Management Console or AWS CLI to get the details of the elastic network interface\.

  In this example, an Amazon EC2 instance in the same VPC uses the private IP of the task\. The application is nginx, but the `server: envoy` header shows that the Service Connect proxy is used\. The Service Connect proxy is listening on the container port from the task definition\.

  ```
  $ curl -v 10.0.19.50:80/
  *   Trying 10.0.19.50:80...
  * Connected to 10.0.19.50 (10.0.19.50) port 80 (#0)
  > GET / HTTP/1.1
  > Host: 10.0.19.50
  > User-Agent: curl/7.79.1
  > Accept: */*
  >
  * Mark bundle as not supporting multiuse
  < HTTP/1.1 200 OK
  < server: envoy
  < date: Tue, 23 Aug 2022 03:53:06 GMT
  < content-type: text/html
  < content-length: 612
  < last-modified: Tue, 16 Apr 2019 13:08:19 GMT
  < etag: "5cb5d3c3-264"
  < accept-ranges: bytes
  < x-envoy-upstream-service-time: 0
  <
  <!DOCTYPE html>
  <html>
  <head>
  <title>Welcome to nginx!</title>
  <style>
      body {
          width: 35em;
          margin: 0 auto;
          font-family: Tahoma, Verdana, Arial, sans-serif;
      }
  </style>
  </head>
  <body>
  <h1>Welcome to nginx!</h1>
  <p>If you see this page, the nginx web server is successfully installed and
  working. Further configuration is required.</p>
  
  <p>For online documentation and support please refer to
  <a href="http://nginx.org/">nginx.org</a>.<br/>
  Commercial support is available at
  <a href="http://nginx.com/">nginx.com</a>.</p>
  
  <p><em>Thank you for using nginx.</em></p>
  </body>
  </html>
  ```

**To view Service Connect metrics**  
The Service Connect proxy creates application \(HTTP, HTTP2, gRPC, or TCP connection\) metrics in CloudWatch metrics\. When you use the CloudWatch console, see the additional metric dimensions of **DiscoveryName**, \(**DiscoveryName, ServiceName, ClusterName**\), **TargetDiscoveryName**, and \(**TargetDiscoveryName, ServiceName, ClusterName**\) under the ECS namespace\. For more information about these metrics and the dimensions, see [Available metrics and dimensions](cloudwatch-metrics.md#available_cloudwatch_metrics)\.