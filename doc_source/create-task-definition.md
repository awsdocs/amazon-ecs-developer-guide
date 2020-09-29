# Creating a task definition<a name="create-task-definition"></a>

Before you can run Docker containers on Amazon ECS, you must create a task definition\. You can define multiple containers and data volumes in a task definition\. For more information about the parameters available in a task definition, see [Task definition parameters](task_definition_parameters.md)\.

**To create a new task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **task definitions**, **Create new task definition**\.

1. On the **Select compatibilities** page, select the launch type that your task should use and choose **Next step**\.
**Note**  
The Fargate launch type is not compatible with Windows containers\.

1. Follow the steps under one of the following tabs, according to the launch type that you have chosen\.

------
#### [ Fargate launch type ]

**Using the Fargate launch type compatibility template**

If you chose **Fargate**, complete the following steps:

1. \(Optional\) If you have a JSON representation of your task definition, complete the following steps:

   1. On the **Configure task and container definitions** page, scroll to the bottom of the page and choose **Configure via JSON**\.

   1. Paste your task definition JSON into the text area and choose **Save**\.

   1. Verify your information and choose **Create**\.

   Scroll to the bottom of the page and choose **Configure via JSON**\.

1. For **Task Definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. \(Optional\) For **Task Role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS API operations on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.
**Note**  
Only roles that have the **Amazon EC2 Container Service Task Role** trust relationship are shown here\. For more information about creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

1. For **Task execution IAM role**, either select your task execution role or choose **Create new role** so that the console can create one for you\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

1. For **Task size**, choose a value for **Task memory \(GB\)** and **Task CPU \(vCPU\)**\. The table below shows the valid combinations\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)

1. For each container in your task definition, complete the following steps:

   1. Choose **Add container**\.

   1. Fill out each required field and any optional fields to use in your container definitions\. More container definition parameters are available in the **Advanced container configuration** menu\. For more information, see [Task definition parameters](task_definition_parameters.md)\.

   1. Choose **Add** to add your container to the task definition\.

1. \(Optional\) For **Service Integration**, to configure the parameters for App Mesh integration, choose **Enable App Mesh integration** and then do the following:

   1. For **Mesh name**, choose the existing App Mesh service mesh to use\. If you don't see any meshes listed, then you need to create one first\. For more information, see [Service meshes](https://docs.aws.amazon.com/app-mesh/latest/userguide/meshes.html) in the *AWS App Mesh User Guide*\.

   1. For **App Mesh endpoints**, select one of the following options\.
      + **Virtual node** – Enter or select the following information\.
        + For **Application container name**, choose the container name to use for the App Mesh integration\. This container must already be defined within the task definition\.
        + For **Virtual node name**, choose the existing App Mesh virtual node to use\. If you don't see any virtual nodes listed, then you need to create one first\. For more information, see [Virtual nodes](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_nodes.html) in the *AWS App Mesh User Guide*\.
        + For **Virtual node port** – Pre\-populated with the listener port set on the virtual node in App Mesh\.
      + **Virtual gateway** – Enter or select the following information\.
        + For **Virtual gateway name**, choose the existing App Mesh virtual gateway to use\. If you don't see any virtual gateways listed, then you need to create one first\. For more information, see [Virtual gateways](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_gateways.html) in the *AWS App Mesh User Guide*\.
        + For **Virtual gateway port** – Pre\-populated with the listener port set on the virtual gateway in App Mesh\.

   1. For **Envoy image**, enter *840364872350*\.dkr\.ecr\.*us\-west\-2*\.amazonaws\.com/aws\-appmesh\-envoy:v1\.15\.0\.0\-prod for all regions except `me-south-1` and `ap-east-1`\. You can replace *us\-west\-2* wih any Region except `me-south-1` and `ap-east-1`\. If your application is in one of these regions, then you also need to replace *840364872350* with the appropriate value for your Region\. For more information, see [Envoy image](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html) in the *AWS App Mesh User Guide*\.

   1. Choose **Apply** and then choose **Confirm**\. This will add an Envoy proxy container to the task definition, as well as the settings to support it\. If you selected **Virtual node**, it will also auto\-populate the App Mesh **Proxy Configuration** settings for the next step\. If you selected **Virtual gateway**, then the **Proxy Configuration** is disabled, because it's not used for a virtual gateway\.

1. \(Optional\) If you selected **Virtual node** in **Service Integration**, then for **Proxy Configuration**, verify all of the pre\-populated values\. For more information about these fields, see the JSON tab in [Update services](https://docs.aws.amazon.com/AmazonECS/latest/userguide/appmesh-getting-started.html#update-services)\.

1. \(Optional\) For **Log Router Integration**, you can add a custom log routing configuration\. Choose **Enable FireLens integration** and then do the following:

   1. For **Type**, choose the log router type to use\.

   1. For **Image**, type the image URI for your log router container\. If you chose the `fluentbit` log router type, the **Image** field prepopulates with the AWS for Fluent Bit image\. For more information, see [Using the AWS for Fluent Bit image](using_firelens.md#firelens-using-fluentbit)\.

   1. Choose **Apply**\. This creates a new log router container to the task definition named `log_router`, and applies the settings to support it\. If you make changes to the log router integration fields, choose **Apply** again to update the FireLens container\.

1. \(Optional\) To define data volumes for your task, choose **Add volume**\. For more information, see [Using data volumes in tasks](using_data_volumes.md)\.

   1. For **Name**, type a name for your volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. In the **Tags** section, specify the key and value for each tag to associate with the task definition\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. Choose **Create**\.

------
#### [ EC2 launch type ]

**Using the EC2 launch type compatibility template**

If you chose **EC2**, complete the following steps:

1. \(Optional\) If you have a JSON representation of your task definition, complete the following steps:

   1. On the **Configure task and container definitions** page, scroll to the bottom of the page and choose **Configure via JSON**\.

   1. Paste your task definition JSON into the text area and choose **Save**\.

   1. Verify your information and choose **Create**\.

   Scroll to the bottom of the page and choose **Configure via JSON**\.

1. For **task definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. \(Optional\) For **Task Role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS APIs on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

   For tasks that use the EC2 launch type, these permissions are usually granted by the Amazon ECS Container Instance IAM role\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.
**Note**  
Only roles that have the **Amazon EC2 Container Service Task Role** trust relationship are shown here\. For more information about creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

1. \(Optional\) For **Network Mode**, choose the Docker network mode to use for the containers in your task\. The available network modes correspond to those described in [Network settings](https://docs.docker.com/engine/reference/run/#/network-settings) in the Docker run reference\. If you select **Enable App Mesh integration** in a following step, then you must select `awsvpc`\.

   The default Docker network mode is `bridge`\. If the network mode is set to `none`, you can't specify port mappings in your container definitions, and the task's containers do not have external connectivity\. If the network mode is `awsvpc`, the task is allocated an elastic network interface\. The `host` and `awsvpc` network modes offer the highest networking performance for containers because they use the Amazon EC2 network stack instead of the virtualized network stack provided by the `bridge` mode; however, exposed container ports are mapped directly to the corresponding host port, so you cannot take advantage of dynamic host port mappings or run multiple instantiations of the same task on a single container instance if port mappings are used\.

1. \(Optional\) For **Task execution role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS APIs on your behalf\.

   For tasks that use the EC2 launch type, these permissions are usually granted by the Amazon ECS Container Instance IAM role, which is specified earlier as the **Task Role**\. There is no need to specify a task execution role\. For more information, see[Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

1. \(Optional\) For **Task size**, choose a value for **Task memory \(GB\)** and **Task CPU \(vCPU\)**\. Supported `Task CPU (vCPU)` values are between 128 CPU units \(0\.125 vCPUs\) and 10240 CPU units \(10 vCPUs\)\.
**Note**  
Task\-level CPU and memory parameters are ignored for Windows containers\. We recommend specifying container\-level resources for Windows containers\.

1. For each container in your task definition, complete the following steps\.

   1. Choose **Add container**\.

   1. Fill out each required field and any optional fields to use in your container definitions \(more container definition parameters are available in the **Advanced container configuration** menu\)\. For more information, see [Task definition parameters](task_definition_parameters.md)\.

   1. Choose **Add** to add your container to the task definition\.

1. \(Optional\) For **Constraint**, define how tasks that are created from this task definition are placed in your cluster\. For tasks that use the EC2 launch type, you can use constraints to place tasks based on Availability Zone, instance type, or custom attributes\. For more information, see [Amazon ECS task placement constraints](task-placement-constraints.md)\.

1. \(Optional\) For **Service Integration**, to configure the parameters for App Mesh integration, choose **Enable App Mesh integration** and then do the following:

   1. For **Mesh name**, choose the existing App Mesh service mesh to use\. If you don't see any meshes listed, then you need to create one first\. For more information, see [Service meshes](https://docs.aws.amazon.com/app-mesh/latest/userguide/meshes.html) in the *AWS App Mesh User Guide*\.

   1. For **App Mesh endpoints**, select one of the following options\.
      + **Virtual node** – Enter or select the following information\.
        + For **Application container name**, choose the container name to use for the App Mesh integration\. This container must already be defined within the task definition\.
        + For **Virtual node name**, choose the existing App Mesh virtual node to use\. If you don't see any virtual nodes listed, then you need to create one first\. For more information, see [Virtual nodes](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_nodes.html) in the *AWS App Mesh User Guide*\.
        + For **Virtual node port** – Pre\-populated with the listener port set on the virtual node in App Mesh\.
      + **Virtual gateway** – Enter or select the following information\.
        + For **Virtual gateway name**, choose the existing App Mesh virtual gateway to use\. If you don't see any virtual gateways listed, then you need to create one first\. For more information, see [Virtual gateways](https://docs.aws.amazon.com/app-mesh/latest/userguide/virtual_gateways.html) in the *AWS App Mesh User Guide*\.
        + For **Virtual gateway port** – Pre\-populated with the listener port set on the virtual gateway in App Mesh\.

   1. For **Envoy image**, enter *840364872350*\.dkr\.ecr\.*us\-west\-2*\.amazonaws\.com/aws\-appmesh\-envoy:v1\.15\.0\.0\-prod for all regions except `me-south-1` and `ap-east-1`\. You can replace *us\-west\-2* wih any Region except `me-south-1` and `ap-east-1`\. If your application is in one of these regions, then you also need to replace *840364872350* with the appropriate value for your Region\. For more information, see [Envoy image](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html) in the *AWS App Mesh User Guide*\.

   1. Choose **Apply** and then choose **Confirm**\. This will add an Envoy proxy container to the task definition, as well as the settings to support it\. If you selected **Virtual node**, it will also auto\-populate the App Mesh **Proxy Configuration** settings for the next step\. If you selected **Virtual gateway**, then the **Proxy Configuration** is disabled, because it's not used for a virtual gateway\.

1. \(Optional\) If you selected **Virtual node** in **Service Integration**, then for **Proxy Configuration**, verify all of the pre\-populated values\. For more information about these fields, see the JSON tab in [Update services](https://docs.aws.amazon.com/AmazonECS/latest/userguide/appmesh-getting-started.html#update-services)\.

1. \(Optional\) For **Log Router Integration**, you can add a custom log routing configuration\. Choose **Enable FireLens integration** and then do the following:

   1. For **Type**, choose the log router type to use\.

   1. For **Image**, type the image URI for your log router container\. If you chose the `fluentbit` log router type, the **Image** field prepopulates with the AWS for Fluent Bit image\. For more information, see [Using the AWS for Fluent Bit image](using_firelens.md#firelens-using-fluentbit)\.

   1. Choose **Apply**\. This creates a new log router container to the task definition named `log_router`, and applies the settings to support it\. If you make changes to the log router integration fields, choose **Apply** again to update the FireLens container\.

1. \(Optional\) To define data volumes for your task, choose **Add volume**\. You can create either a bind mount or Docker volume\. For more information, see [Using data volumes in tasks](using_data_volumes.md)\.

   1. For **Name**, type a name for your volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. \(Optional\) To create a bind mount volume, for **Source path**, type the path on the host container instance to present to the container\. If you leave this field empty, the Docker daemon assigns a host path for you\. If you specify a source path, the data volume persists at the specified location on the host container instance until you delete it manually\. If the source path does not exist on the host container instance, the Docker daemon creates it\. If the location does exist, the contents of the source path folder are exported to the container\.

   1. To create a Docker volume, select **Specify a volume driver**\.

      1. For **Driver**, choose the Docker volume driver to use\. The driver value must match the driver name provided by Docker\. Use `docker plugin ls` on your container instance to retrieve the driver name\.

      1. For **Scope**, choose the option that determines the lifecycle of the Docker volume\. Docker volumes that are scoped to a `task` are automatically provisioned when the task starts and destroyed when the task stops\. Docker volumes that are scoped as `shared` persist after the task stops\.

      1. Select **Enable auto\-provisioning** to have the Docker volume created if it does not already exist\. This option is only available for volumes that specify the `shared` scope\.

      1. For **Driver options**, specify the driver\-specific key values to use\.

      1. For **Volume labels**, specify the custom metadata to add to your Docker volume\.

1. In the **Tags** section, specify the key and value for each tag to associate with the task definition\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. Choose **Create**\.

------

## Task definition template<a name="task-definition-template"></a>

An empty task definition template is shown below\. You can use this template to create your task definition, which can then be pasted into the console JSON input area or saved to a file and used with the AWS CLI `--cli-input-json` option\. For more information, see [Task definition parameters](task_definition_parameters.md)\.

```
{
    "family": "",
    "taskRoleArn": "",
    "executionRoleArn": "",
    "networkMode": "awsvpc",
    "containerDefinitions": [
        {
            "name": "",
            "image": "",
            "repositoryCredentials": {
                "credentialsParameter": ""
            },
            "cpu": 0,
            "memory": 0,
            "memoryReservation": 0,
            "links": [
                ""
            ],
            "portMappings": [
                {
                    "containerPort": 0,
                    "hostPort": 0,
                    "protocol": "tcp"
                }
            ],
            "essential": true,
            "entryPoint": [
                ""
            ],
            "command": [
                ""
            ],
            "environment": [
                {
                    "name": "",
                    "value": ""
                }
            ],
            "environmentFiles": [
                {
                    "value": "",
                    "type": "s3"
                }
            ],
            "mountPoints": [
                {
                    "sourceVolume": "",
                    "containerPath": "",
                    "readOnly": true
                }
            ],
            "volumesFrom": [
                {
                    "sourceContainer": "",
                    "readOnly": true
                }
            ],
            "linuxParameters": {
                "capabilities": {
                    "add": [
                        ""
                    ],
                    "drop": [
                        ""
                    ]
                },
                "devices": [
                    {
                        "hostPath": "",
                        "containerPath": "",
                        "permissions": [
                            "read"
                        ]
                    }
                ],
                "initProcessEnabled": true,
                "sharedMemorySize": 0,
                "tmpfs": [
                    {
                        "containerPath": "",
                        "size": 0,
                        "mountOptions": [
                            ""
                        ]
                    }
                ],
                "maxSwap": 0,
                "swappiness": 0
            },
            "secrets": [
                {
                    "name": "",
                    "valueFrom": ""
                }
            ],
            "dependsOn": [
                {
                    "containerName": "",
                    "condition": "HEALTHY"
                }
            ],
            "startTimeout": 0,
            "stopTimeout": 0,
            "hostname": "",
            "user": "",
            "workingDirectory": "",
            "disableNetworking": true,
            "privileged": true,
            "readonlyRootFilesystem": true,
            "dnsServers": [
                ""
            ],
            "dnsSearchDomains": [
                ""
            ],
            "extraHosts": [
                {
                    "hostname": "",
                    "ipAddress": ""
                }
            ],
            "dockerSecurityOptions": [
                ""
            ],
            "interactive": true,
            "pseudoTerminal": true,
            "dockerLabels": {
                "KeyName": ""
            },
            "ulimits": [
                {
                    "name": "msgqueue",
                    "softLimit": 0,
                    "hardLimit": 0
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "KeyName": ""
                },
                "secretOptions": [
                    {
                        "name": "",
                        "valueFrom": ""
                    }
                ]
            },
            "healthCheck": {
                "command": [
                    ""
                ],
                "interval": 0,
                "timeout": 0,
                "retries": 0,
                "startPeriod": 0
            },
            "systemControls": [
                {
                    "namespace": "",
                    "value": ""
                }
            ],
            "resourceRequirements": [
                {
                    "value": "",
                    "type": "GPU"
                }
            ],
            "firelensConfiguration": {
                "type": "fluentd",
                "options": {
                    "KeyName": ""
                }
            }
        }
    ],
    "volumes": [
        {
            "name": "",
            "host": {
                "sourcePath": ""
            },
            "dockerVolumeConfiguration": {
                "scope": "task",
                "autoprovision": true,
                "driver": "",
                "driverOpts": {
                    "KeyName": ""
                },
                "labels": {
                    "KeyName": ""
                }
            },
            "efsVolumeConfiguration": {
                "fileSystemId": "",
                "rootDirectory": "",
                "transitEncryption": "ENABLED",
                "transitEncryptionPort": 0,
                "authorizationConfig": {
                    "accessPointId": "",
                    "iam": "ENABLED"
                }
            }
        }
    ],
    "placementConstraints": [
        {
            "type": "memberOf",
            "expression": ""
        }
    ],
    "requiresCompatibilities": [
        "EC2"
    ],
    "cpu": "",
    "memory": "",
    "tags": [
        {
            "key": "",
            "value": ""
        }
    ],
    "pidMode": "task",
    "ipcMode": "none",
    "proxyConfiguration": {
        "type": "APPMESH",
        "containerName": "",
        "properties": [
            {
                "name": "",
                "value": ""
            }
        ]
    },
    "inferenceAccelerators": [
        {
            "deviceName": "",
            "deviceType": ""
        }
    ]
}
```

You can generate this task definition template using the following AWS CLI command:

```
aws ecs register-task-definition --generate-cli-skeleton
```