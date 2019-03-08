# ecs\-cli compose service rm, delete, down<a name="cmd-ecs-cli-compose-service-rm"></a>

Updates the desired count of the service to `0` and then deletes the service\.

## Syntax<a name="cmd-ecs-cli-compose-service-rm-syntax"></a>

This command accepts `rm`, `delete`, or `down` when used\.

ecs\-cli compose service rm\|delete\|down \[\-\-timeout *value*\] \[\-\-delete\-namespace\] \[\-\-help\]

## Options<a name="cmd-ecs-cli-compose-service-rm-options"></a>


| Name | Description | 
| --- | --- | 
|  `--timeout value`  |  Specifies the timeout value, in minutes \(decimals supported\), to wait for the running task count to change\. If the running task count has not changed for the specified period of time, the Amazon ECS CLI times out and returns an error\. Setting the timeout to `0` causes the command to return without checking for success\. The default timeout value is `5` \(minutes\)\. Default value: `5` Required: No  | 
|  `--delete-namespace`  |  If specified, the private namespace created with either the compose service create or compose service up commands is deleted\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-compose-service-rm-examples"></a>

### Example 1<a name="cmd-ecs-cli-compose-service-rm-example-1"></a>

This example scales the service created by the `hello-world` project to a desired count of `0` and then deletes the service\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml service rm
```

Output:

```
INFO[0000] Updated ECS service successfully              desiredCount=0 serviceName=ecscompose-service-hello-world
INFO[0000] Service status                                desiredCount=0 runningCount=2 serviceName=ecscompose-service-hello-world
INFO[0015] Service status                                desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
INFO[0015] (service ecscompose-service-hello-world) has stopped 2 running tasks: (task 682dc22f-8bfa-4c28-b6f8-3a916bd8f86a) (task 80602da8-442c-48ea-a8a9-80328c302b89).  timestamp=2017-08-18 21:25:28 +0000 UTC
INFO[0015] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
INFO[0015] Deleted ECS service                           service=ecscompose-service-hello-world
INFO[0015] ECS Service has reached a stable state        desiredCount=0 runningCount=0 serviceName=ecscompose-service-hello-world
```