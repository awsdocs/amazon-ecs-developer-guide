# Amazon ECS Log File Locations<a name="logs"></a>

Amazon ECS stores logs in the `/var/log/ecs` folder of your container instances\. There are logs available from the Amazon ECS container agent and the `ecs-init` service that controls the state of the agent \(start/stop\) on the container instance\. You can view these log files by connecting to a container instance using SSH\. For more information, see [Connect to Your Container Instance](instance-connect.md)\.

**Note**  
If you are unsure how to collect all of the various logs on your container instances, you can use the Amazon ECS logs collector\. For more information, see [Amazon ECS Logs Collector](ecs-logs-collector.md)\.

## Amazon ECS Container Agent Log<a name="agent-logs"></a>

The Amazon ECS container agent stores logs at `/var/log/ecs/ecs-agent.log.timestamp` on Linux instances, and `C:\ProgramData\Amazon\ECS\log\ecs-agent.log.timestamp` on Windows instances\.

**Note**  
You can increase the verbosity of the container agent logs by setting `ECS_LOGLEVEL=debug` and restarting the container agent\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

```
cat /var/log/ecs/ecs-agent.log.2016-08-15-15
```

Output:

```
2016-08-15T15:54:41Z [INFO] Starting Agent: Amazon ECS Agent - v1.12.0 (895f3c1)
2016-08-15T15:54:41Z [INFO] Loading configuration
2016-08-15T15:54:41Z [WARN] Invalid value for task cleanup duration, will be overridden to 3h0m0s, parsed value 0, minimum threshold 1m0s
2016-08-15T15:54:41Z [INFO] Checkpointing is enabled. Attempting to load state
2016-08-15T15:54:41Z [INFO] Loading state! module="statemanager"
2016-08-15T15:54:41Z [INFO] Detected Docker versions [1.17 1.18 1.19 1.20 1.21 1.22]
2016-08-15T15:54:41Z [INFO] Registering Instance with ECS
2016-08-15T15:54:41Z [INFO] Registered! module="api client"
```

## Amazon ECS `ecs-init` Log<a name="ecs-init-logs"></a>

The `ecs-init` process stores logs at `/var/log/ecs/ecs-init.log`\.

```
cat /var/log/ecs/ecs-init.log
```

Output:

```
2018-02-16T18:13:54Z [INFO] pre-start
2018-02-16T18:13:56Z [INFO] start
2018-02-16T18:13:56Z [INFO] No existing agent container to remove.
2018-02-16T18:13:56Z [INFO] Starting Amazon Elastic Container Service Agent
```

## IAM Roles for Tasks Credential Audit Log<a name="task_iam_roles-logs"></a>

When the credential provider for the IAM role is used to provide credentials to tasks, these requests are logged in `/var/log/ecs/audit.log.YYYY-MM-DD-HH`\.

The log entry format is as follows:

+ Time stamp

+ HTTP response code

+ IP address and port number of request origin

+ Relative URI of the credential provider

+ The user agent that made the request

+ The ARN of the task to which the requesting container belongs

+ The `GetCredentials` API name and version number

+ The name of the Amazon ECS cluster to which the container instance is registered

+ The container instance ARN

An example log entry is shown below:

```
cat /var/log/ecs/audit.log.2016-07-13-16
```

Output:

```
2016-07-13T16:11:53Z 200 172.17.0.5:52444 "/v1/credentials" "python-requests/2.7.0 CPython/2.7.6 Linux/4.4.14-24.50.amzn1.x86_64" TASK_ARN GetCredentials 1 CLUSTER_NAME CONTAINER_INSTANCE_ARN
```