# Tutorial: Creating a Cluster with a EC2 Task Using the AWS CLI<a name="ECS_AWSCLI_EC2"></a>

The following steps will help you set up a cluster, register a task definition, run a task, and perform other common scenarios in Amazon ECS with the AWS CLI\. Ensure you are using the latest version of the AWS CLI\. For more information on how to upgrade to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

[Prerequisites](#AWSCLI_EC2_prereq)

[Step 1: \(Optional\) Create a Cluster](#AWSCLI_EC2_create_cluster)

[Step 2: Launch an Instance with the Amazon ECS AMI](#AWSCLI_EC2_launch_container_instance)

[Step 3: List Container Instances](#AWSCLI_EC2_list_container_instances)

[Step 4: Describe your Container Instance](#AWSCLI_EC2_describe_container_instance)

[Step 5: Register a Task Definition](#AWSCLI_EC2_register_task_definition)

[Step 6: List Task Definitions](#AWSCLI_EC2_list_task_definitions)

[Step 7: Run a Task](#AWSCLI_EC2_run_task)

[Step 8: List Tasks](#AWSCLI_EC2_list_tasks)

[Step 9: Describe the Running Task](#AWSCLI_EC2_describe_task)

## Prerequisites<a name="AWSCLI_EC2_prereq"></a>

This tutorial assumes the following prerequisites have been completed:
+ The latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading your AWS CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ The steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard](IAMPolicyExamples.md#first-run-permissions) IAM policy example\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

## Step 1: \(Optional\) Create a Cluster<a name="AWSCLI_EC2_create_cluster"></a>

By default, your account receives a `default` cluster when you launch your first container instance\.

**Note**  
The benefit of using the `default` cluster that is provided for you is that you don't have to specify the `--cluster cluster_name` option in the subsequent commands\. If you do create your own, non\-default, cluster you need to specify `--cluster cluster_name` for each command that you intend to use with that cluster\.

Create your own cluster with a unique name with the following command:

```
aws ecs create-cluster --cluster-name MyCluster
```

Output:

```
{
    "cluster": {
        "clusterName": "MyCluster",
        "status": "ACTIVE",
        "clusterArn": "arn:aws:ecs:region:aws_account_id:cluster/MyCluster"
    }
}
```

## Step 2: Launch an Instance with the Amazon ECS AMI<a name="AWSCLI_EC2_launch_container_instance"></a>

You must have an Amazon ECS container instance in your cluster before you can run tasks on it\. If you do not have any container instances in your cluster, see [Launching an Amazon ECS Container Instance](launch_container_instance.md) for more information\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0a0d2004b44b9287c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0a0d2004b44b9287c) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0254e5972ebcd132c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0254e5972ebcd132c) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-093381d21a4fc38d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-093381d21a4fc38d1) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0de5608ca20c07aa2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0de5608ca20c07aa2) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-024c0b7d07abc6526 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-024c0b7d07abc6526) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-005307409c5f6e76c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-005307409c5f6e76c) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0dbcd2533bc72c3f6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0dbcd2533bc72c3f6) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-03804565a6baf6d30 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-03804565a6baf6d30) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0060ad36f655af38b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0060ad36f655af38b) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0d5f884dada5562c6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0d5f884dada5562c6) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0aa8b7a8042811ddf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0aa8b7a8042811ddf) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-065c0bd2832a70f9d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-065c0bd2832a70f9d) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0d50dee936e241e7e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0d50dee936e241e7e) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-056a07eb5b1d13734 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-056a07eb5b1d13734) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-0078e33a9103e1e58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0078e33a9103e1e58) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.f\-amazon\-ecs\-optimized | ami\-a842dcc9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-a842dcc9) | 

## Step 3: List Container Instances<a name="AWSCLI_EC2_list_container_instances"></a>

Within a few minutes of launching your container instance, the Amazon ECS agent registers the instance with your default cluster\. You can list the container instances in a cluster by running the following command:

```
aws ecs list-container-instances --cluster default
```

Output:

```
{
    "containerInstanceArns": [
        "arn:aws:ecs:us-east-1:aws_account_id:container-instance/container_instance_ID"
    ]
}
```

## Step 4: Describe your Container Instance<a name="AWSCLI_EC2_describe_container_instance"></a>

After you have the ARN or ID of a container instance, you can use the describe\-container\-instances command to get valuable information on the instance, such as remaining and registered CPU and memory resources\.

```
aws ecs describe-container-instances --cluster default --container-instances container_instance_ID
```

Output:

```
{
    "failures": [],
    "containerInstances": [
        {
            "status": "ACTIVE",
            "registeredResources": [
                {
                    "integerValue": 1024,
                    "longValue": 0,
                    "type": "INTEGER",
                    "name": "CPU",
                    "doubleValue": 0.0
                },
                {
                    "integerValue": 995,
                    "longValue": 0,
                    "type": "INTEGER",
                    "name": "MEMORY",
                    "doubleValue": 0.0
                },
                {
                    "name": "PORTS",
                    "longValue": 0,
                    "doubleValue": 0.0,
                    "stringSetValue": [
                        "22",
                        "2376",
                        "2375",
                        "51678"
                    ],
                    "type": "STRINGSET",
                    "integerValue": 0
                },
                {
                    "name": "PORTS_UDP",
                    "longValue": 0,
                    "doubleValue": 0.0,
                    "stringSetValue": [],
                    "type": "STRINGSET",
                    "integerValue": 0
                }
            ],
            "ec2InstanceId": "instance_id",
            "agentConnected": true,
            "containerInstanceArn": "arn:aws:ecs:us-west-2:aws_account_id:container-instance/container_instance_ID",
            "pendingTasksCount": 0,
            "remainingResources": [
                {
                    "integerValue": 1024,
                    "longValue": 0,
                    "type": "INTEGER",
                    "name": "CPU",
                    "doubleValue": 0.0
                },
                {
                    "integerValue": 995,
                    "longValue": 0,
                    "type": "INTEGER",
                    "name": "MEMORY",
                    "doubleValue": 0.0
                },
                {
                    "name": "PORTS",
                    "longValue": 0,
                    "doubleValue": 0.0,
                    "stringSetValue": [
                        "22",
                        "2376",
                        "2375",
                        "51678"
                    ],
                    "type": "STRINGSET",
                    "integerValue": 0
                },
                {
                    "name": "PORTS_UDP",
                    "longValue": 0,
                    "doubleValue": 0.0,
                    "stringSetValue": [],
                    "type": "STRINGSET",
                    "integerValue": 0
                }
            ],
            "runningTasksCount": 0,
            "attributes": [
                {
                    "name": "com.amazonaws.ecs.capability.privileged-container"
                },
                {
                    "name": "com.amazonaws.ecs.capability.docker-remote-api.1.17"
                },
                {
                    "name": "com.amazonaws.ecs.capability.docker-remote-api.1.18"
                },
                {
                    "name": "com.amazonaws.ecs.capability.docker-remote-api.1.19"
                },
                {
                    "name": "com.amazonaws.ecs.capability.logging-driver.json-file"
                },
                {
                    "name": "com.amazonaws.ecs.capability.logging-driver.syslog"
                }
            ],
            "versionInfo": {
                "agentVersion": "1.5.0",
                "agentHash": "b197edd",
                "dockerVersion": "DockerVersion: 1.7.1"
            }
        }
    ]
}
```

You can also find the Amazon EC2 instance ID that you can use to monitor the instance in the Amazon EC2 console or with the aws ec2 describe\-instances \-\-instance\-id *instance\_id* command\.

## Step 5: Register a Task Definition<a name="AWSCLI_EC2_register_task_definition"></a>

Before you can run a task on your ECS cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example is a simple task definition that uses a `busybox` image from Docker Hub and simply sleeps for 360 seconds\. For more information about the available task definition parameters, 

```
{
  "containerDefinitions": [
    {
      "name": "sleep",
      "image": "busybox",
      "cpu": 10,
      "command": [
        "sleep",
        "360"
      ],
      "memory": 10,
      "essential": true
    }
  ],
  "family": "sleep360"
}
```

The above example JSON can be passed to the AWS CLI in two ways: you can save the task definition JSON as a file and pass it with the `--cli-input-json file://path_to_file.json` option, or you can escape the quotation marks in the JSON and pass the JSON container definitions on the command line as in the below example\. If you choose to pass the container definitions on the command line, your command additionally requires a `--family` parameter that is used to keep multiple versions of your task definition associated with each other\.

To use a JSON file for container definitions:

```
aws ecs register-task-definition --cli-input-json file://$HOME/tasks/sleep360.json
```

To use a JSON string for container definitions:

```
aws ecs register-task-definition --family sleep360 --container-definitions "[{\"name\":\"sleep\",\"image\":\"busybox\",\"cpu\":10,\"command\":[\"sleep\",\"360\"],\"memory\":10,\"essential\":true}]"
```

The register\-task\-definition returns a description of the task definition after it completes its registration\.

```
{
    "taskDefinition": {
        "volumes": [],
        "taskDefinitionArn": "arn:aws:ec2:us-east-1:aws_account_id:task-definition/sleep360:1",
        "containerDefinitions": [
            {
                "environment": [],
                "name": "sleep",
                "mountPoints": [],
                "image": "busybox",
                "cpu": 10,
                "portMappings": [],
                "command": [
                    "sleep",
                    "360"
                ],
                "memory": 10,
                "essential": true,
                "volumesFrom": []
            }
        ],
        "family": "sleep360",
        "revision": 1
    }
}
```

## Step 6: List Task Definitions<a name="AWSCLI_EC2_list_task_definitions"></a>

You can list the task definitions for your account at any time with the list\-task\-definitions command\. The output of this command shows the `family` and `revision` values that you can use together when calling run\-task or start\-task\.

```
aws ecs list-task-definitions
```

Output:

```
{
    "taskDefinitionArns": [
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/sleep300:1",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/sleep300:2",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/sleep360:1",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/wordpress:3",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/wordpress:4",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/wordpress:5",
        "arn:aws:ec2:us-east-1:aws_account_id:task-definition/wordpress:6"
    ]
}
```

## Step 7: Run a Task<a name="AWSCLI_EC2_run_task"></a>

After you have registered a task for your account and have launched a container instance that is registered to your cluster, you can run the registered task in your cluster\. For this example, you place a single instance of the `sleep360:1` task definition in your default cluster\.

```
aws ecs run-task --cluster default --task-definition sleep360:1 --count 1
```

Output:

```
{
    "tasks": [
        {
            "taskArn": "arn:aws:ecs:us-east-1:aws_account_id:task/task_ID",
            "overrides": {
                "containerOverrides": [
                    {
                        "name": "sleep"
                    }
                ]
            },
            "lastStatus": "PENDING",
            "containerInstanceArn": "arn:aws:ecs:us-east-1:aws_account_id:container-instance/container_instance_ID",
            "clusterArn": "arn:aws:ecs:us-east-1:aws_account_id:cluster/default",
            "desiredStatus": "RUNNING",
            "taskDefinitionArn": "arn:aws:ecs:us-east-1:aws_account_id:task-definition/sleep360:1",
            "containers": [
                {
                    "containerArn": "arn:aws:ecs:us-east-1:aws_account_id:container/container_ID",
                    "taskArn": "arn:aws:ecs:us-east-1:aws_account_id:task/task_ID",
                    "lastStatus": "PENDING",
                    "name": "sleep"
                }
            ]
        }
    ]
}
```

## Step 8: List Tasks<a name="AWSCLI_EC2_list_tasks"></a>

List the tasks for your cluster\. You should see the task that you ran in the previous section\. You can take the task ID or the full ARN that is returned from this command and use it to describe the task later\.

```
aws ecs list-tasks --cluster default
```

Output:

```
{
    "taskArns": [
        "arn:aws:ecs:us-east-1:aws_account_id:task/task_ID"
    ]
}
```

## Step 9: Describe the Running Task<a name="AWSCLI_EC2_describe_task"></a>

Describe the task using the task ID retrieved earlier to get more information about the task\.

```
aws ecs describe-tasks --cluster default --task task_ID
```

Output:

```
{
    "failures": [],
    "tasks": [
        {
            "taskArn": "arn:aws:ecs:us-east-1:aws_account_id:task/task_ID",
            "overrides": {
                "containerOverrides": [
                    {
                        "name": "sleep"
                    }
                ]
            },
            "lastStatus": "RUNNING",
            "containerInstanceArn": "arn:aws:ecs:us-east-1:aws_account_id:container-instance/container_instance_ID",
            "clusterArn": "arn:aws:ecs:us-east-1:aws_account_id:cluster/default",
            "desiredStatus": "RUNNING",
            "taskDefinitionArn": "arn:aws:ecs:us-east-1:aws_account_id:task-definition/sleep360:1",
            "containers": [
                {
                    "containerArn": "arn:aws:ecs:us-east-1:aws_account_id:container/container_ID",
                    "taskArn": "arn:aws:ecs:us-east-1:aws_account_id:task/task_ID",
                    "lastStatus": "RUNNING",
                    "name": "sleep",
                    "networkBindings": []
                }
            ]
        }
    ]
}
```