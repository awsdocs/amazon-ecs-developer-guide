# ecs\-cli compose service stop<a name="cmd-ecs-cli-compose-service-stop"></a>

Stops the running tasks that belong to the service created with the compose project\. This command updates the desired count of the service to `0`\.

The `--timeout` option specifies the timeout value, in minutes \(decimals supported\), to wait for the running task count to change\. If the running task count has not changed for the specified period of time, the Amazon ECS CLI times out and returns an error\. Setting the timeout to `0` causes the command to return without checking for success\. The default timeout value is `5` \(minutes\)\.

## Syntax<a name="cmd-ecs-cli-compose-service-stop-syntax"></a>

ecs\-cli compose service stop \[\-\-timeout *value*\] \[\-\-help\]

## Options<a name="cmd-ecs-cli-compose-service-stop-options"></a>


| Name | Description | 
| --- | --- | 
|  `--timeout value`  |  Specifies the timeout value, in minutes \(decimals supported\), to wait for the running task count to change\. If the running task count has not changed for the specified period of time, the Amazon ECS CLI times out and returns an error\. Setting the timeout to `0` causes the command to return without checking for success\. The default timeout value is `5` \(minutes\)\. Default value: `5` Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 