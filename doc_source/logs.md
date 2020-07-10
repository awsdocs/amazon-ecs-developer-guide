# Amazon ECS Log File Locations<a name="logs"></a>

Amazon ECS stores logs in the `/var/log/ecs` folder of your container instances\. There are logs available from the Amazon ECS container agent and from the `ecs-init` service that controls the state of the agent \(start/stop\) on the container instance\. You can view these log files by connecting to a container instance using SSH\. For more information, see [Connect to your container instance](instance-connect.md)\.

**Note**  
If you are not sure how to collect all of the logs on your container instances, you can use the Amazon ECS logs collector\. For more information, see [Amazon ECS logs collector](ecs-logs-collector.md)\.

## Amazon ECS Container Agent Log<a name="agent-logs"></a>

The Amazon ECS container agent stores logs on your container instances\.

For container agent version 1\.36\.0 and later, by default the logs are located at `/var/log/ecs/ecs-agent.log` on Linux instances and at `C:\ProgramData\Amazon\ECS\log\ecs-agent.log` on Windows instances\.

For container agent version 1\.35\.0 and earlier, by default the logs are located at `/var/log/ecs/ecs-agent.log.timestamp` on Linux instances and at `C:\ProgramData\Amazon\ECS\log\ecs-agent.log.timestamp` on Windows instances\.

By default, the agent logs are rotated hourly with a maximum of 24 logs being stored\.

The following are the container agent configuration variables that can be used to change the default agent logging behavior\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

`ECS_LOGFILE`  
Example values: `/ecs-agent.log`  
Default value on Linux: Null  
Default value on Windows: Null  
The location where agent logs should be written\. If you are running the agent via `ecs-init`, which is the default method when using the Amazon ECS\-optimized AMI, the in\-container path will be `/log` and `ecs-init` mounts that out to `/var/log/ecs/` on the host\.

`ECS_LOGLEVEL`  
Example values: `crit`, `error`, `warn`, `info`, `debug`  
Default value on Linux: `info`  
Default value on Windows: `info`  
The level of detail to log\.

`ECS_LOG_ROLLOVER_TYPE`  
Example values: `size`, `hourly`  
Default value on Linux: `hourly`  
Default value on Windows: `hourly`  
Determines whether the container agent log file will be rotated hourly or based on size\. By default, the agent log file is rotated each hour\.

`ECS_LOG_OUTPUT_FORMAT`  
Example values: `logfmt`, `json`  
Default value on Linux: `logfmt`  
Default value on Windows: `logfmt`  
Determines the log output format\. When the `json` format is used, each line in the log will be a structured JSON map\.

`ECS_LOG_MAX_FILE_SIZE_MB`  
Example values: `10`  
Default value on Linux: `10`  
Default value on Windows: `10`  
When the `ECS_LOG_ROLLOVER_TYPE` variable is set to `size`, this variable determines the maximum size \(in MB\) of the log file before it is rotated\. If the rollover type is set to `hourly`, then this variable is ignored\.

`ECS_LOG_MAX_ROLL_COUNT`  
Example values: `24`  
Default value on Linux: `24`  
Default value on Windows: `24`  
Determines the number of rotated log files to keep\. Older log files are deleted once this limit is reached\.

For container agent version 1\.36\.0 and later, the following is an example log file when the `logfmt` format is used\.

```
level=info time=2019-12-12T23:43:29Z msg="Loading configuration" module=agent.go
level=info time=2019-12-12T23:43:29Z msg="Image excluded from cleanup: amazon/amazon-ecs-agent:latest" module=parse.go
level=info time=2019-12-12T23:43:29Z msg="Image excluded from cleanup: amazon/amazon-ecs-pause:0.1.0" module=parse.go
level=info time=2019-12-12T23:43:29Z msg="Amazon ECS agent Version: 1.36.0, Commit: ca640387" module=agent.go
level=info time=2019-12-12T23:43:29Z msg="Creating root ecs cgroup: /ecs" module=init_linux.go
level=info time=2019-12-12T23:43:29Z msg="Creating cgroup /ecs" module=cgroup_controller_linux.go
level=info time=2019-12-12T23:43:29Z msg="Loading state!" module=statemanager.go
level=info time=2019-12-12T23:43:29Z msg="Event stream ContainerChange start listening..." module=eventstream.go
level=info time=2019-12-12T23:43:29Z msg="Restored cluster 'auto-robc'" module=agent.go
level=info time=2019-12-12T23:43:29Z msg="Restored from checkpoint file. I am running as 'arn:aws:ecs:us-west-2:0123456789:container-instance/auto-robc/3330a8a91d15464ea30662d5840164cd' in cluster 'auto-robc'" module=agent.go
```

The following is an example log file when the JSON format is used\.

```
{"time": "2019-11-07T22:52:02Z", "level": "info", "msg": "Starting Amazon Elastic Container Service Agent", "module": "engine.go"}
```

For container agent versions 1\.35\.0 and earlier, the following is the format of the log file\.

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

When the credential provider for the IAM role is used to provide credentials to tasks, these requests are saved in an audit log\. The audit log inherits the same log rotation settings as the container agent log\. The `ECS_LOG_ROLLOVER_TYPE`, `ECS_LOG_MAX_FILE_SIZE_MB`, and `ECS_LOG_MAX_ROLL_COUNT` container agent configuration variables can be set to affect the behavior of the audit log\. For more information, see [Amazon ECS Container Agent Log](#agent-logs)\.

For container agent version 1\.36\.0 and later, the audit log is located at `/var/log/ecs/audit.log`\. When the log is rotated, a timestamp in `YYYY-MM-DD-HH` format is added to the end of the log file name\.

For container agent version 1\.35\.0 and earlier, the audit log is located at `/var/log/ecs/audit.log.YYYY-MM-DD-HH`\.

The log entry format is as follows:
+ Timestamp
+ HTTP response code
+ IP address and port number of request origin
+ Relative URI of the credential provider
+ The user agent that made the request
+ The ARN of the task to which the requesting container belongs
+ The `GetCredentials` API name and version number
+ The name of the Amazon ECS cluster to which the container instance is registered
+ The container instance ARN

An example log entry is shown below\.

```
cat /var/log/ecs/audit.log.2016-07-13-16
```

Output:

```
2016-07-13T16:11:53Z 200 172.17.0.5:52444 "/v1/credentials" "python-requests/2.7.0 CPython/2.7.6 Linux/4.4.14-24.50.amzn1.x86_64" TASK_ARN GetCredentials 1 CLUSTER_NAME CONTAINER_INSTANCE_ARN
```