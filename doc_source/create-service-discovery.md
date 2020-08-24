# Tutorial: Creating a service using Service Discovery<a name="create-service-discovery"></a>

Service discovery has been integrated into the Create Service wizard in the Amazon ECS console\. For more information, see [Creating a service](create-service.md)\.

The following tutorial shows how to create an ECS service containing a Fargate task that uses service discovery with the AWS CLI\.

For a list of Regions that support service discovery, see [Service Discovery](service-discovery.md)\.

Fargate tasks are only supported in the following Regions:


| Region Name | Region | 
| --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  US West \(N\. California\)  |  us\-west\-1 \(`usw1-az1` & `usw1-az3` only\)  | 
|  US West \(Oregon\)  |  us\-west\-2  | 
|  Africa \(Cape Town\)  |  af\-south\-1  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1 \(`apne1-az1`, `apne1-az2`, & `apne1-az4` only\)  | 
|  Canada \(Central\)  |  ca\-central\-1 \(`cac1-az1` & `cac1-az2` only\)  | 
|  China \(Beijing\)  |  cn\-north\-1 \(`cnn1-az1` & `cnn1-az2` only\)  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | 
|  Europe \(London\)  |  eu\-west\-2  | 
|  Europe \(Paris\)  |  eu\-west\-3  | 
|  Europe \(Milan\)  |  eu\-south\-1  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | 
|  South America \(São Paulo\)  |  sa\-east\-1  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  | 

## Prerequisites<a name="create-service-discovery-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The latest version of the AWS CLI is installed and configured\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ The steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

## Step 1: Create the Service Discovery resources<a name="create-service-discovery-namespace"></a>

Use the following steps to create your service discovery namespace and service discovery service\.

**To create the Service Discovery resources**

1. Create a private service discovery namespace named `tutorial` within one of your existing VPCs:

   ```
   aws servicediscovery create-private-dns-namespace --name tutorial --vpc vpc-abcd1234 --region us-east-1
   ```

   Output:

   ```
   {
       "OperationId": "h2qe3s6dxftvvt7riu6lfy2f6c3jlhf4-je6chs2e"
   }
   ```

1. Using the `OperationId` from the previous output, verify that the private namespace was created successfully\. Copy the namespace ID as it is used in subsequent commands\.

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

1. Using the `NAMESPACE` ID from the previous output, create a service discovery service named `myapplication`\. Copy the service discovery service ID as it is used in subsequent commands:

   ```
   aws servicediscovery create-service --name myapplication --dns-config 'NamespaceId="ns-uejictsjen2i4eeg",DnsRecords=[{Type="A",TTL="300"}]' --health-check-custom-config FailureThreshold=1 --region us-east-1
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

## Step 2: Create the Amazon ECS resources<a name="create-service-discovery-cluster"></a>

Use the following steps to create your Amazon ECS cluster, task definition, and service\.

**To create Amazon ECS resources**

1. Create an Amazon ECS cluster named `tutorial` to use:

   ```
   aws ecs create-cluster --cluster-name tutorial --region us-east-1
   ```

   Output:

   ```
   {
       "cluster": {
           "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/tutorial",
           "clusterName": "tutorial",
           "status": "ACTIVE",
           "registeredContainerInstancesCount": 0,
           "runningTasksCount": 0,
           "pendingTasksCount": 0,
           "activeServicesCount": 0,
           "statistics": []
       }
   }
   ```

1. Register a task definition that is compatible with Fargate\. It requires the use of the `awsvpc` network mode\. The following is the example task definition used for this tutorial\.

   First, create a file named `fargate-task.json` with the contents of the following task definition:

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

   Then, register the task definition using the `fargate-task.json` file that you created:

   ```
   aws ecs register-task-definition --cli-input-json file://fargate-task.json --region us-east-1
   ```

1. Create a file named `ecs-service-discovery.json` with the contents of the ECS service that you are going to create\. This example uses the task definition created in the previous step\. An `awsvpcConfiguration` is required because the example task definition uses the `awsvpc` network mode\.

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

   Create your ECS service, specifying the Fargate launch type and the `LATEST` platform version, which supports service discovery:

   ```
   aws ecs create-service --cli-input-json file://ecs-service-discovery.json --region us-east-1
   ```

   Output:

   ```
   {
       "service": {
           "serviceArn": "arn:aws:ecs:region:aws_account_id:service/ecs-service-discovery",
           "serviceName": "ecs-service-discovery",
           "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/tutorial",
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
           "platformVersion": "LATEST",
           "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/tutorial-task-def:1",
           "deploymentConfiguration": {
               "maximumPercent": 200,
               "minimumHealthyPercent": 100
           },
           "deployments": [
               {
                   "id": "ecs-svc/9223370516993140842",
                   "status": "PRIMARY",
                   "taskDefinition": "arn:aws:ecs:region:aws_account_id:task-definition/tutorial-task-def:1",
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

## Step 3: Verify Service Discovery<a name="create-service-discovery-verify"></a>

You can verify that everything has been created properly by querying your service discovery information\. After service discovery is configured, you can query it using either the AWS Cloud Map API operations or by using `dig` from within your VPC, as described below\.

**To verify service discovery configuration**

1. Using the service discovery service ID, list the service discovery instances:

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

1. Using the service discovery namespace and service, use additional parameters to query the details about the service discovery instances:

   ```
   aws servicediscovery discover-instances --namespace-name tutorial --service-name myapplication --query-parameters ECS_CLUSTER_NAME=tutorial --region us-east-1
   ```

   Output:

   ```
   {
       "Instances": [
           {
               "InstanceId": "16becc26-8558-4af1-9fbd-f81be062a266", 
               "NamespaceName": "tutorial", 
               "ServiceName": "ecs-service-discovery", 
               "HealthStatus": "HEALTHY", 
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

1. The DNS records created in the Route 53 hosted zone for the service discovery service can be queried with the following AWS CLI commands\.

   Using the namespace ID, get information about the namespace, which includes the Route 53 hosted zone ID:

   ```
   aws servicediscovery get-namespace --id ns-uejictsjen2i4eeg --region us-east-1
   ```

   Output:

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

1. Using the Route 53 hosted zone ID, get the resource record set for the hosted zone:

   ```
   aws route53 list-resource-record-sets --hosted-zone-id Z35JQ4ZFDRYPLV --region us-east-1
   ```

   Output:

   ```
   {
       "ResourceRecordSets": [
           {
               "Name": "tutorial.",
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
               "Name": "tutorial.",
               "Type": "SOA",
               "TTL": 900,
               "ResourceRecords": [
                   {
                       "Value": "ns-1536.awsdns-00.co.uk. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400"
                   }
               ]
           },
           {
               "Name": "myapplication.tutorial.",
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

1. You can also query the DNS using `dig` from an instance within your VPC with the following command:

   ```
   dig +short myapplication.tutorial
   ```

   Output:

   ```
   172.31.87.2
   ```

## Step 4: Clean up<a name="create-service-discovery-cleanup"></a>

When you are finished with this tutorial, you should clean up the associated resources to avoid incurring charges for unused resources\.

**To clean up the service discovery instances and Amazon ECS resources**

1. Deregister the service discovery service instances:

   ```
   aws servicediscovery deregister-instance --service-id srv-utcrh6wavdkggqtk --instance-id 16becc26-8558-4af1-9fbd-f81be062a266 --region us-east-1
   ```

   Output:

   ```
   {
       "OperationId": "xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv"
   }
   ```

1. Using the `OperationId` from the previous output, verify that the service discovery service instances were deregistered successfully:

   ```
   aws servicediscovery get-operation --operation-id xhu73bsertlyffhm3faqi7kumsmx274n-jh0zimzv --region us-east-1
   ```

   Output:

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

1. Delete the service discovery service:

   ```
   aws servicediscovery delete-service --id srv-utcrh6wavdkggqtk --region us-east-1
   ```

1. Delete the service discovery namespace:

   ```
   aws servicediscovery delete-namespace --id ns-uejictsjen2i4eeg --region us-east-1
   ```

   Output:

   ```
   {
       "OperationId": "c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj"
   }
   ```

1. Using the `OperationId` from the previous output, verify that the service discovery namespace was deleted successfully:

   ```
   aws servicediscovery get-operation --operation-id c3ncqglftesw4ibgj5baz6ktaoh6cg4t-jh0ztysj --region us-east-1
   ```

   Output:

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

1. Update the Amazon ECS service so that the desired count is `0`, which allows you to delete it:

   ```
   aws ecs update-service --cluster tutorial --service ecs-service-discovery --desired-count 0 --force-new-deployment --region us-east-1
   ```

1. Delete the Amazon ECS service:

   ```
   aws ecs delete-service --cluster tutorial --service ecs-service-discovery --region us-east-1
   ```

1. Delete the Amazon ECS cluster:

   ```
   aws ecs delete-cluster --cluster tutorial --region us-east-1
   ```