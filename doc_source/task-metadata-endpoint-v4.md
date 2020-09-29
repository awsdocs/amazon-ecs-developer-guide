# Task metadata endpoint version 4<a name="task-metadata-endpoint-v4"></a>

The Amazon ECS container agent injects an environment variable into each container, referred to as the *task metadata endpoint* which provides various task metadata and [Docker stats](https://docs.docker.com/engine/api/v1.30/#operation/ContainerStats) to the container\.

The task metadata and network rate stats are sent to CloudWatch Container Insights and can be viewed in the AWS Management Console\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.

**Important**  
Amazon ECS provided earlier versions of the task metadata endpoint\. To avoid the need to create new task metadata endpoint versions in the future, additional metadata may be added to the version 4 output\. We will not remove any existing metadata or change the metadata field names\.

## Enabling the task metadata endpoint<a name="task-metadata-endpoint-v4-enable"></a>

The environment variable is injected by default into the containers of Amazon ECS tasks on Fargate that use platform version `1.4.0` or later and Amazon ECS tasks on Amazon EC2 that are running at least version `1.39.0` of the Amazon ECS container agent\. For more information, see [Amazon ECS Container Agent Versions](ecs-agent-versions.md)\.

You can add support for this feature on Amazon EC2 instances using older versions of the Amazon ECS container agent by updating the agent to the latest version\. For more information, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

## Task metadata endpoint version 4 paths<a name="task-metadata-endpoint-v4-paths"></a>

The following task metadata endpoint paths are available to containers\.

`${ECS_CONTAINER_METADATA_URI_V4}`  
This path returns metadata for the container\.

`${ECS_CONTAINER_METADATA_URI_V4}/task`  
This path returns metadata for the task, including a list of the container IDs and names for all of the containers associated with the task\. For more information about the response for this endpoint, see [Task metadata JSON response](#task-metadata-endpoint-v4-response)\.

`${ECS_CONTAINER_METADATA_URI_V4}/stats`  
This path returns Docker stats for the specific container\. For more information about each of the returned stats, see [ContainerStats](https://docs.docker.com/engine/api/v1.30/#operation/ContainerStats) in the Docker API documentation\.  
Amazon ECS tasks on AWS Fargate require that the container run for \~1 second prior to returning the container stats\.
For Amazon ECS tasks that use the `awsvpc` or `bridge` network modes hosted on Amazon EC2 instances running at least version `1.43.0` of the container agent, there will be additional network rate stats included in the response\. For all other tasks, the response will only include the cumulative network stats\.

`${ECS_CONTAINER_METADATA_URI_V4}/task/stats`  
This path returns Docker stats for all of the containers associated with the task\. This can be used by sidecar containers to extract network metrics\. For more information about each of the returned stats, see [ContainerStats](https://docs.docker.com/engine/api/v1.30/#operation/ContainerStats) in the Docker API documentation\.  
Amazon ECS tasks on AWS Fargate require that the container run for \~1 second prior to returning the task stats\.
For Amazon ECS tasks that use the `awsvpc` or `bridge` network modes hosted on Amazon EC2 instances running at least version `1.43.0` of the container agent, there will be additional network rate stats included in the response\. For all other tasks, the response will only include the cumulative network stats\.

## Task metadata JSON response<a name="task-metadata-endpoint-v4-response"></a>

The following information is returned from the task metadata endpoint \(`${ECS_CONTAINER_METADATA_URI_V4}/task`\) JSON response\.

`Cluster`  
The Amazon Resource Name \(ARN\) or short name of the Amazon ECS cluster to which the task belongs\.

`TaskARN`  
The full Amazon Resource Name \(ARN\) of the task to which the container belongs\.

`Family`  
The family of the Amazon ECS task definition for the task\.

`Revision`  
The revision of the Amazon ECS task definition for the task\.

`DesiredStatus`  
The desired status for the task from Amazon ECS\.

`KnownStatus`  
The known status for the task from Amazon ECS\.

`Limits`  
The resource limits specified at the task level \(such as CPU and memory\)\. This parameter is omitted if no resource limits are defined\.

`PullStartedAt`  
The timestamp for when the first container image pull began\.

`PullStoppedAt`  
The timestamp for when the last container image pull finished\.

`AvailabilityZone`  
The Availability Zone the task is in\.  
The Availability Zone metadata is only available for Fargate tasks using platform version 1\.4 or later\.

`Containers`  
A list of container metadata for each container associated with the task\.    
`DockerId`  
The Docker ID for the container\.  
`Name`  
The name of the container as specified in the task definition\.  
`DockerName`  
The name of the container supplied to Docker\. The Amazon ECS container agent generates a unique name for the container to avoid name collisions when multiple copies of the same task definition are run on a single instance\.  
`Image`  
The image for the container\.  
`ImageID`  
The SHA\-256 digest for the image\.  
`Ports`  
Any ports exposed for the container\. This parameter is omitted if there are no exposed ports\.  
`Labels`  
Any labels applied to the container\. This parameter is omitted if there are no labels applied\.  
`DesiredStatus`  
The desired status for the container from Amazon ECS\.  
`KnownStatus`  
The known status for the container from Amazon ECS\.  
`ExitCode`  
The exit code for the container\. This parameter is omitted if the container has not exited\.  
`Limits`  
The resource limits specified at the container level \(such as CPU and memory\)\. This parameter is omitted if no resource limits are defined\.  
`CreatedAt`  
The time stamp for when the container was created\. This parameter is omitted if the container has not been created yet\.  
`StartedAt`  
The time stamp for when the container started\. This parameter is omitted if the container has not started yet\.  
`FinishedAt`  
The time stamp for when the container stopped\. This parameter is omitted if the container has not stopped yet\.  
`Type`  
The type of the container\. Containers that are specified in your task definition are of type `NORMAL`\. You can ignore other container types, which are used for internal task resource provisioning by the Amazon ECS container agent\.  
`Networks`  
The network information for the container, such as the network mode and IP address\. This parameter is omitted if no network information is defined\.

`ExecutionStoppedAt`  
The time stamp for when the tasks `DesiredStatus` moved to `STOPPED`\. This occurs when an essential container moves to `STOPPED`\.

## Examples<a name="task-metadata-endpoint-v4-examples"></a>

The following examples show example outputs from each of the task metadata endpoints\.

### Example Container Metadata Response<a name="task-metadata-endpoint-v4-example-container-metadata-response"></a>

When querying the `${ECS_CONTAINER_METADATA_URI_V4}` endpoint you are returned only metadata about the container itself\. The following is an example output\.

```
{
    "DockerId": "c7a6b9b237934e9999f319ea3ccc9da4query-metadata",
    "Name": "query-metadata",
    "DockerName": "query-metadata",
    "Image": "mreferre/eksutils",
    "ImageID": "sha256:1b146e73f801617610dcb00441c6423e7c85a7583dd4a65ed1be03cb0e123311",
    "Labels": {
        "com.amazonaws.ecs.cluster": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:cluster/default",
        "com.amazonaws.ecs.container-name": "query-metadata",
        "com.amazonaws.ecs.task-arn": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:task/default/c7a6b9b237934e9999f319ea3ccc9da4",
        "com.amazonaws.ecs.task-definition-family": "query-metadata",
        "com.amazonaws.ecs.task-definition-version": "3"
    },
    "DesiredStatus": "RUNNING",
    "KnownStatus": "RUNNING",
    "Limits": {
        "CPU": 2
    },
    "CreatedAt": "2020-03-26T22:11:23.62831313Z",
    "StartedAt": "2020-03-26T22:11:23.62831313Z",
    "Type": "NORMAL",
    "Networks": [
        {
            "NetworkMode": "awsvpc",
            "IPv4Addresses": [
                "10.0.0.61"
            ],
            "AttachmentIndex": 0,
            "IPv4SubnetCIDRBlock": "10.0.0.0/24",
            "MACAddress": "0a:d2:d0:80:b6:b4",
            "DomainNameServers": [
                "10.0.0.2"
            ],
            "DomainNameSearchList": [
                "us-west-2.compute.internal"
            ],
            "PrivateDNSName": "ip-10-0-0-61.us-west-2.compute.internal",
            "SubnetGatewayIpv4Address": ""
        }
    ]
}
```

### Example Task Metadata Response<a name="task-metadata-endpoint-v4-example-task-metadata-response"></a>

When querying the `${ECS_CONTAINER_METADATA_URI_V4}/task` endpoint you are returned metadata about the task the container is part of\. The following is an example output\.

```
{
    "Cluster": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:cluster/default",
    "TaskARN": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:task/default/febee046097849aba589d4435207c04a",
    "Family": "query-metadata",
    "Revision": "7",
    "DesiredStatus": "RUNNING",
    "KnownStatus": "RUNNING",
    "Limits": {
        "CPU": 0.25,
        "Memory": 512
    },
    "PullStartedAt": "2020-03-26T22:25:40.420726088Z",
    "PullStoppedAt": "2020-03-26T22:26:22.235177616Z",
    "AvailabilityZone": "us-west-2c",
    "Containers": [
        {
            "DockerId": "febee046097849aba589d4435207c04aquery-metadata",
            "Name": "query-metadata",
            "DockerName": "query-metadata",
            "Image": "mreferre/eksutils",
            "ImageID": "sha256:1b146e73f801617610dcb00441c6423e7c85a7583dd4a65ed1be03cb0e123311",
            "Labels": {
                "com.amazonaws.ecs.cluster": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:cluster/default",
                "com.amazonaws.ecs.container-name": "query-metadata",
                "com.amazonaws.ecs.task-arn": "arn:aws:ecs:us-west-2:&ExampleAWSAccountNo1;:task/default/febee046097849aba589d4435207c04a",
                "com.amazonaws.ecs.task-definition-family": "query-metadata",
                "com.amazonaws.ecs.task-definition-version": "7"
            },
            "DesiredStatus": "RUNNING",
            "KnownStatus": "RUNNING",
            "Limits": {
                "CPU": 2
            },
            "CreatedAt": "2020-03-26T22:26:24.534553758Z",
            "StartedAt": "2020-03-26T22:26:24.534553758Z",
            "Type": "NORMAL",
            "Networks": [
                {
                    "NetworkMode": "awsvpc",
                    "IPv4Addresses": [
                        "10.0.0.108"
                    ],
                    "AttachmentIndex": 0,
                    "IPv4SubnetCIDRBlock": "10.0.0.0/24",
                    "MACAddress": "0a:62:17:7a:36:68",
                    "DomainNameServers": [
                        "10.0.0.2"
                    ],
                    "DomainNameSearchList": [
                        "us-west-2.compute.internal"
                    ],
                    "PrivateDNSName": "ip-10-0-0-108.us-west-2.compute.internal",
                    "SubnetGatewayIpv4Address": ""
                }
            ]
        }
    ]
}
```

### Example Stats Response<a name="task-metadata-endpoint-v4-example-stats-response"></a>

When querying the `${ECS_CONTAINER_METADATA_URI_V4}/stats` endpoint you are returned network metrics for the container\. For Amazon ECS tasks that use the `awsvpc` or `bridge` network modes hosted on Amazon EC2 instances running at least version `1.43.0` of the container agent, there will be additional network rate stats included in the response\. For all other tasks, the response will only include the cumulative network stats\.

The following is an example output from an Amazon ECS task on Amazon EC2 that uses the `bridge` network mode\.

```
{
    "read": "2020-08-06T22:45:31.349948898Z",
    "preread": "2020-08-06T22:45:30.347408046Z",
    "pids_stats": {
        "current": 2
    },
    "blkio_stats": {
        "io_service_bytes_recursive": [
            
        ],
        "io_serviced_recursive": [
            
        ],
        "io_queue_recursive": [
            
        ],
        "io_service_time_recursive": [
            
        ],
        "io_wait_time_recursive": [
            
        ],
        "io_merged_recursive": [
            
        ],
        "io_time_recursive": [
            
        ],
        "sectors_recursive": [
            
        ]
    },
    "num_procs": 0,
    "storage_stats": {
        
    },
    "cpu_stats": {
        "cpu_usage": {
            "total_usage": 412520458,
            "percpu_usage": [
                203701476,
                208818982,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ],
            "usage_in_kernelmode": 10000000,
            "usage_in_usermode": 350000000
        },
        "system_cpu_usage": 2243010000000,
        "online_cpus": 2,
        "throttling_data": {
            "periods": 0,
            "throttled_periods": 0,
            "throttled_time": 0
        }
    },
    "precpu_stats": {
        "cpu_usage": {
            "total_usage": 412420561,
            "percpu_usage": [
                203701476,
                208719085,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ],
            "usage_in_kernelmode": 10000000,
            "usage_in_usermode": 350000000
        },
        "system_cpu_usage": 2241010000000,
        "online_cpus": 2,
        "throttling_data": {
            "periods": 0,
            "throttled_periods": 0,
            "throttled_time": 0
        }
    },
    "memory_stats": {
        "usage": 1896448,
        "max_usage": 6324224,
        "stats": {
            "active_anon": 495616,
            "active_file": 0,
            "cache": 0,
            "dirty": 0,
            "hierarchical_memory_limit": 134217728,
            "hierarchical_memsw_limit": 268435456,
            "inactive_anon": 0,
            "inactive_file": 0,
            "mapped_file": 0,
            "pgfault": 4025,
            "pgmajfault": 0,
            "pgpgin": 2908,
            "pgpgout": 2785,
            "rss": 503808,
            "rss_huge": 0,
            "total_active_anon": 495616,
            "total_active_file": 0,
            "total_cache": 0,
            "total_dirty": 0,
            "total_inactive_anon": 0,
            "total_inactive_file": 0,
            "total_mapped_file": 0,
            "total_pgfault": 4025,
            "total_pgmajfault": 0,
            "total_pgpgin": 2908,
            "total_pgpgout": 2785,
            "total_rss": 503808,
            "total_rss_huge": 0,
            "total_unevictable": 0,
            "total_writeback": 0,
            "unevictable": 0,
            "writeback": 0
        },
        "limit": 134217728
    },
    "name": "query-metadata",
    "id": "1823e1f6-7248-43c3-bed6-eea1fa7501a5query-metadata",
    "networks": {
        "eth0": {
            "rx_bytes": 1802,
            "rx_packets": 19,
            "rx_errors": 0,
            "rx_dropped": 0,
            "tx_bytes": 567,
            "tx_packets": 7,
            "tx_errors": 0,
            "tx_dropped": 0
        }
    },
    "network_rate_stats": {
        "rx_bytes_per_sec": 0,
        "tx_bytes_per_sec": 0
    }
}
```

### Example Task Stats Response<a name="task-metadata-endpoint-v4-example-task-stats-response"></a>

When querying the `${ECS_CONTAINER_METADATA_URI_V4}/task/stats` endpoint you are returned network metrics about the task the container is part of\. The following is an example output from a Fargate task\.

```
{
    "1823e1f6-7248-43c3-bed6-eea1fa7501a5query-metadata": {
        "read": "2020-04-06T16:12:01.090148907Z",
        "preread": "2020-04-06T16:11:56.083890951Z",
        "pids_stats": {
            
        },
        "blkio_stats": {
            "io_service_bytes_recursive": [
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Read",
                    "value": 3452928
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Write",
                    "value": 0
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Sync",
                    "value": 3452928
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Async",
                    "value": 0
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Total",
                    "value": 3452928
                }
            ],
            "io_serviced_recursive": [
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Read",
                    "value": 118
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Write",
                    "value": 0
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Sync",
                    "value": 118
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Async",
                    "value": 0
                },
                {
                    "major": 202,
                    "minor": 26368,
                    "op": "Total",
                    "value": 118
                }
            ],
            "io_queue_recursive": [
                
            ],
            "io_service_time_recursive": [
                
            ],
            "io_wait_time_recursive": [
                
            ],
            "io_merged_recursive": [
                
            ],
            "io_time_recursive": [
                
            ],
            "sectors_recursive": [
                
            ]
        },
        "num_procs": 0,
        "storage_stats": {
            
        },
        "cpu_stats": {
            "cpu_usage": {
                "total_usage": 410557100,
                "percpu_usage": [
                    410557100,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ],
                "usage_in_kernelmode": 10000000,
                "usage_in_usermode": 250000000
            },
            "throttling_data": {
                "periods": 0,
                "throttled_periods": 0,
                "throttled_time": 0
            }
        },
        "precpu_stats": {
            "cpu_usage": {
                "total_usage": 0,
                "usage_in_kernelmode": 0,
                "usage_in_usermode": 0
            },
            "throttling_data": {
                "periods": 0,
                "throttled_periods": 0,
                "throttled_time": 0
            }
        },
        "memory_stats": {
            "usage": 4390912,
            "max_usage": 6488064,
            "stats": {
                "active_anon": 278528,
                "active_file": 344064,
                "cache": 3452928,
                "dirty": 0,
                "hierarchical_memory_limit": 536870912,
                "hierarchical_memsw_limit": 9223372036854772000,
                "inactive_anon": 0,
                "inactive_file": 3108864,
                "mapped_file": 2412544,
                "pgfault": 2800,
                "pgmajfault": 28,
                "pgpgin": 3144,
                "pgpgout": 2233,
                "rss": 278528,
                "rss_huge": 0,
                "total_active_anon": 278528,
                "total_active_file": 344064,
                "total_cache": 3452928,
                "total_dirty": 0,
                "total_inactive_anon": 0,
                "total_inactive_file": 3108864,
                "total_mapped_file": 2412544,
                "total_pgfault": 2800,
                "total_pgmajfault": 28,
                "total_pgpgin": 3144,
                "total_pgpgout": 2233,
                "total_rss": 278528,
                "total_rss_huge": 0,
                "total_unevictable": 0,
                "total_writeback": 0,
                "unevictable": 0,
                "writeback": 0
            },
            "limit": 9223372036854772000
        },
        "name": "query-metadata",
        "id": "1823e1f6-7248-43c3-bed6-eea1fa7501a5query-metadata",
        "networks": {
            "eth1": {
                "rx_bytes": 564655295,
                "rx_packets": 384960,
                "rx_errors": 0,
                "rx_dropped": 0,
                "tx_bytes": 3043269,
                "tx_packets": 54355,
                "tx_errors": 0,
                "tx_dropped": 0
            }
        }
    }
}
```