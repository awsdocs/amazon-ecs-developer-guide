# Creating a Task Definition<a name="create-task-definition"></a>

Before you can run Docker containers on Amazon ECS, you must create a task definition\. You can define multiple containers and data volumes in a task definition\. For more information about the parameters available in a task definition, see [Task Definition Parameters](task_definition_parameters.md)\.

**To create a new task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. On the **Select compatibilities** page, select the launch type that your task should use and choose **Next step**\.
**Note**  
The Fargate launch type is not compatible with Windows containers\.

1. \(Optional\) If you have a JSON representation of your task definition, complete the following steps:

   1. On the **Configure task and container definitions** page, scroll to the bottom of the page and choose **Configure via JSON**\.

   1. Paste your task definition JSON into the text area and choose **Save**\.

   1. Verify your information and choose **Create**\.

   Scroll to the bottom of the page and choose **Configure via JSON**\.

1. Follow the steps under one of the following tabs, according to the launch type that you have chosen\.

------
#### [ Fargate launch type ]

**Using the Fargate launch type compatibility template**

If you chose **Fargate**, complete the following steps:

1. For **Task Definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. \(Optional\) For **Task Role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS APIs on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.
**Note**  
Only roles that have the **Amazon EC2 Container Service Task Role** trust relationship are shown here\. For help creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

1. For **Task execution IAM role**, either select your task execution role or select **Create new role** so the console can create one for you\. For more information, see[Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

1. For **Task size**, choose a value for **Task memory \(GB\)** and **Task CPU \(vCPU\)**\. The table below shows the valid combinations\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)

1. For each container in your task definition, complete the following steps\.

   1. Choose **Add container**\.

   1. Fill out each required field and any optional fields to use in your container definitions \(more container definition parameters are available in the **Advanced container configuration** menu\)\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

   1. Choose **Add** to add your container to the task definition\.

1. \(Optional\) To define data volumes for your task, choose **Add volume**\. For more information, see [Using Data Volumes in Tasks](using_data_volumes.md)\.

   1. For **Name**, type a name for your volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. Choose **Create**\.

------
#### [ EC2 launch type ]

**Using the EC2 launch type compatibility template**

If you chose **EC2**, complete the following steps:

1. For **Task Definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. \(Optional\) For **Task Role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS APIs on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

   For tasks that use the EC2 launch type, these permissions are usually granted by the Amazon ECS Container Instance IAM role\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.
**Note**  
Only roles that have the **Amazon EC2 Container Service Task Role** trust relationship are shown here\. For more information about creating an IAM role for your tasks, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.

1. \(Optional\) For **Network Mode**, choose the Docker network mode to use for the containers in your task\. The available network modes correspond to those described in [Network settings](https://docs.docker.com/engine/reference/run/#/network-settings) in the Docker run reference\.

   The default Docker network mode is `bridge`\. If the network mode is set to `none`, you can't specify port mappings in your container definitions, and the task's containers do not have external connectivity\. If the network mode is `awsvpc`, the task is allocated an elastic network interface\. The `host` and `awsvpc` network modes offer the highest networking performance for containers because they use the Amazon EC2 network stack instead of the virtualized network stack provided by the `bridge` mode; however, exposed container ports are mapped directly to the corresponding host port, so you cannot take advantage of dynamic host port mappings or run multiple instantiations of the same task on a single container instance if port mappings are used\.

1. \(Optional\) For **Task execution role**, choose an IAM role that provides permissions for containers in your task to make calls to AWS APIs on your behalf\.

   For tasks that use the EC2 launch type, these permissions are usually granted by the Amazon ECS Container Instance IAM role which is specified earlier as the **Task Role** so there is no need to specify a task execution role\. For more information, see[Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

1. \(Optional\) For **Task size**, choose a value for **Task memory \(GB\)** and **Task CPU \(vCPU\)**\. Supported `Task CPU (vCPU)` values are between 128 CPU units \(0\.125 vCPUs\) and 10240 CPU units \(10 vCPUs\)\.
**Note**  
Task\-level CPU and memory parameters are ignored for Windows containers\. We recommend specifying container\-level resources for Windows containers\.

1. For each container in your task definition, complete the following steps\.

   1. Choose **Add container**\.

   1. Fill out each required field and any optional fields to use in your container definitions \(more container definition parameters are available in the **Advanced container configuration** menu\)\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

   1. Choose **Add** to add your container to the task definition\.

1. \(Optional\) For **Constraint**, define how tasks that are created from this task definition are placed in your cluster\. For tasks that use the EC2 launch type, you can use constraints to place tasks based on Availability Zone, instance type, or custom attributes\. For more information, see [Amazon ECS Task Placement Constraints](task-placement-constraints.md)\.

1. \(Optional\) To define data volumes for your task, choose **Add volume**\. You can create either a bind mount or Docker volume\. For more information, see [Using Data Volumes in Tasks](using_data_volumes.md)\.

   1. For **Name**, type a name for your volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. \(Optional\) To create a bind mount volume, for **Source path**, type the path on the host container instance to present to the container\. If you leave this field empty, the Docker daemon assigns a host path for you\. If you specify a source path, the data volume persists at the specified location on the host container instance until you delete it manually\. If the source path does not exist on the host container instance, the Docker daemon creates it\. If the location does exist, the contents of the source path folder are exported to the container\.

   1. To create a Docker volume, select **Specify a volume driver**\.

      1. For **Driver**, choose the Docker volume driver to use\. The driver value must match the driver name provided by Docker\. Use `docker plugin ls` on your container instance to retrieve the driver name\.

      1. For **Scope**, choose the option that determines the lifecycle of the Docker volume\. Docker volumes that are scoped to a `task` are automatically provisioned when the task starts and destroyed when the task stops\. Docker volumes that are scoped as `shared` persist after the task stops\.

      1. Select **Enable auto\-provisioning** to have the Docker volume created if it does not already exist\. This option is only available for volumes that specify the `shared` scope\.

      1. For **Driver options**, specify the driver\-specific key values to use\.

      1. For **Volume labels**, specify the custom metadata to add to your Docker volume\.

1. Choose **Create**\.

------

## Task Definition Template<a name="task-definition-template"></a>

An empty task definition template is shown below\. You can use this template to create your task definition, which can then be pasted into the console JSON input area or saved to a file and used with the AWS CLI `--cli-input-json` option\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

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
                            "mknod"
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
                ]
            },
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
            "dockerLabels": {
                "KeyName": ""
            },
            "ulimits": [
                {
                    "name": "rttime",
                    "softLimit": 0,
                    "hardLimit": 0
                }
            ],
            "logConfiguration": {
                "logDriver": "json-file",
                "options": {
                    "KeyName": ""
                }
            },
            "healthCheck": {
                "command": [
                    ""
                ],
                "interval": 0,
                "timeout": 0,
                "retries": 0,
                "startPeriod": 0
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
                "scope": "shared",
                "autoprovision": true,
                "driver": "",
                "driverOpts": {
                    "KeyName": ""
                },
                "labels": {
                    "KeyName": ""
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
    "memory": ""
}
```

You can generate this task definition template using the following AWS CLI command:

```
aws ecs register-task-definition --generate-cli-skeleton
```