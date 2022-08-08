# Tutorial: Creating a service using Service Discovery<a name="create-service-discovery"></a>

Service discovery is now integrated into the Create Service wizard in the Amazon ECS console\. For more information, see [Creating an Amazon ECS service](create-service.md)\.

The following tutorial shows how to create an ECS service containing a Fargate task that uses service discovery with the AWS CLI\.

For a list of AWS Regions that support service discovery, see [Service Discovery](service-discovery.md)\.

For information about the Regions that support Fargate, see [Supported Regions for Amazon ECS on AWS Fargate](AWS_Fargate-Regions.md)\.

## Prerequisites<a name="create-service-discovery-prereqs"></a>

Before you start this tutorial, make sure that the following prerequisites are met:
+ The latest version of the AWS CLI is installed and configured\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\.
+ The steps described in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) are complete\.
+ Your AWS user has the required permissions specified in the [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ You have created at least one VPC and one security group\. For more information, see [Create a virtual private cloud](get-set-up-for-amazon-ecs.md#create-a-vpc)\.

## Step 1: Create the Service Discovery resources in AWS Cloud Map<a name="create-service-discovery-namespace"></a>

Follow these steps to create your service discovery namespace and service discovery service:

1. Create a private Cloud Map service discovery namespace\. This example creates a namespace that's called `tutorial`\. Replace *vpc\-abcd1234* with the ID of one of your existing VPCs\. 

   ```
   aws servicediscovery create-private-dns-namespace \
         --name tutorial \
         --vpc vpc-abcd1234
   ```

   The output of this command is as follows\.

   ```
   {
       "OperationId": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
   }
   ```

1. Using the `OperationId` from the output of the previous step, verify that the private namespace was created successfully\. Make note of the namespace ID because you use it in subsequent commands\.

   ```
   aws servicediscovery get-operation \
         --operation-id h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e
   ```

   The output is as follows\.

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

1. Using the `NAMESPACE` ID from the output of the previous step, create a service discovery service\. This example creates a service named `myapplication`\. Make note of the service ID and ARN because you use them in subsequent commands\.

   ```
   aws servicediscovery create-service \
         --name myapplication \
         --dns-config "NamespaceId="ns-uejictsjen2i4eeg",DnsRecords=[{Type="A",TTL="300"}]" \
         --health-check-custom-config FailureThreshold=1
   ```

   The output is as follows\.

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

## Step 2: Create the Amazon ECS resources<a name="create-service-discovery-cluster"></a>

Follow these steps to create your Amazon ECS cluster, task definition, and service:

1. Create an Amazon ECS cluster\. This example creates a cluster that's named `tutorial`\. 

   ```
   aws ecs create-cluster \
         --cluster-name tutorial
   ```

1. Register a task definition that's compatible with Fargate and uses the `awsvpc` network mode\. Follow these steps:

   1. Create a file that's named `fargate-task.json` with the contents of the following task definition\.

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
      ```

   1. Register the task definition using `fargate-task.json`\.

      ```
      aws ecs register-task-definition \
            --cli-input-json file://fargate-task.json
      ```

1. Create an ECS service by following these steps:

   1. Create a file that's named `ecs-service-discovery.json` with the contents of the ECS service that you're creating\. This example uses the task definition that was created in the previous step\. An `awsvpcConfiguration` is required because the example task definition uses the `awsvpc` network mode\. 

      When you create the ECS service, specify the Fargate launch type, and the `LATEST` platform version that supports service discovery\. When the service discovery service is created in AWS Cloud Map , `registryArn` is the ARN returned\. The `securityGroups` and `subnets` must belong to the VPC that's used to create the Cloud Map namespace\. You can obtain the security group and subnet IDs from the Amazon VPC Console\.

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
          "platformVersion": "LATEST",
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

   1. Create your ECS service using `ecs-service-discovery.json`\.

      ```
      aws ecs create-service \
            --cli-input-json file://ecs-service-discovery.json
      ```

## Step 3: Verify Service Discovery in AWS Cloud Map<a name="create-service-discovery-verify"></a>

You can verify that everything is created properly by querying your service discovery information\. After service discovery is configured, you can either use AWS Cloud Map API operations, or call `dig` from an instance within your VPC\. Follow these steps:

1. Using the service discovery service ID, list the service discovery instances\. Make note of the instance ID \(marked in bold\) for resource cleanup\. 

   ```
    aws servicediscovery list-instances \
          --service-id srv-utcrh6wavdkggqtk
   ```

   The output is as follows\.

   ```
   {
       "Instances": [
           {
               "Id": "16becc26-8558-4af1-9fbd-f81be062a266",
               "Attributes": {
                   "AWS_INSTANCE_IPV4": "172.31.87.2"
                   "AWS_INSTANCE_PORT": "80", 
                   "AVAILABILITY_ZONE": "us-east-1a", 
                   "REGION": "us-east-1", 
                   "ECS_SERVICE_NAME": "ecs-service-discovery", 
                   "ECS_CLUSTER_NAME": "tutorial", 
                   "ECS_TASK_DEFINITION_FAMILY": "tutorial-task-def"
               }
           }
       ]
   }
   ```

1. Use the service discovery namespace, service, and additional parameters such as ECS cluster name to query details about the service discovery instances\.

   ```
   aws servicediscovery discover-instances \
         --namespace-name tutorial \
         --service-name myapplication \
         --query-parameters ECS_CLUSTER_NAME=tutorial
   ```

1. The DNS records that are created in the Route 53 hosted zone for the service discovery service can be queried with the following AWS CLI commands:

   1. Using the namespace ID, get information about the namespace, which includes the Route 53 hosted zone ID\.

      ```
      aws servicediscovery \
            get-namespace --id ns-uejictsjen2i4eeg
      ```

      The output is as follows\.

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
      ```

   1. Using the Route 53 hosted zone ID from the previous step \(see the text in bold\), get the resource record set for the hosted zone\. 

      ```
      aws route53 list-resource-record-sets \
            --hosted-zone-id Z35JQ4ZFDRYPLV
      ```

1. You can also query the DNS from an instance within your VPC using `dig`\.

   ```
   dig +short myapplication.tutorial
   ```

## Step 4: Clean up<a name="create-service-discovery-cleanup"></a>

When you're finished with this tutorial, clean up the associated resources to avoid incurring charges for unused resources\. Follow these steps:

1. Deregister the service discovery service instances using the service ID and instance ID that you noted previously\.

   ```
   aws servicediscovery deregister-instance \
         --service-id srv-utcrh6wavdkggqtk \
         --instance-id 16becc26-8558-4af1-9fbd-f81be062a266
   ```

   The output is as follows\.

   ```
   {
       "OperationId": "xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv"
   }
   ```

1. Using the `OperationId` from the output of the previous step, verify that the service discovery service instances were deregistered successfully\.

   ```
   aws servicediscovery get-operation \ 
         --operation-id xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv
   ```

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
   ```

1. Delete the service discovery service using the service ID\.

   ```
   aws servicediscovery delete-service \ 
         --id srv-utcrh6wavdkggqtk
   ```

1. Delete the service discovery namespace using the namespace ID\.

   ```
   aws servicediscovery delete-namespace \ 
         --id ns-uejictsjen2i4eeg
   ```

   The output is as follows\.

   ```
   {
       "OperationId": "c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj"
   }
   ```

1. Using the `OperationId` from the output of the previous step, verify that the service discovery namespace was deleted successfully\.

   ```
   aws servicediscovery get-operation \ 
         --operation-id c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj
   ```

   The output is as follows\.

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
   ```

1. Update the desired count for the Amazon ECS service to `0`\. You must do this to delete the service in the next step\.

   ```
   aws ecs update-service \
         --cluster tutorial \
         --service ecs-service-discovery \
         --desired-count 0
   ```

1. Delete the Amazon ECS service\.

   ```
   aws ecs delete-service \
         --cluster tutorial \
         --service ecs-service-discovery
   ```

1. Delete the Amazon ECS cluster\.

   ```
   aws ecs delete-cluster \
         --cluster tutorial
   ```