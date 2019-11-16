# ecs\-cli compose service start<a name="cmd-ecs-cli-compose-service-start"></a>

Starts one copy of each of the containers on the created Amazon ECS service\. This command updates the desired count of the service to `1`\.

## Syntax<a name="cmd-ecs-cli-compose-service-start-syntax"></a>

ecs\-cli compose service start \[\-\-create\-log\-groups\] \[\-\-force\-deployment\] \[\-\-help\]

## Options<a name="cmd-ecs-cli-compose-service-start-options"></a>


| Name | Description | 
| --- | --- | 
|  `--timeout value`  |  Specifies the timeout value, in minutes \(decimals supported\), to wait for the running task count to change\. If the running task count has not changed for the specified period of time, the Amazon ECS CLI times out and returns an error\. Setting the timeout to `0` causes the command to return without checking for success\. The default timeout value is `5` \(minutes\)\. Default value: `5` Required: No  | 
|  `--create-log-groups`  |  Creates the CloudWatch log groups specified in your Compose files\. Required: No  | 
|  `--force-deployment`  |  Forces a new deployment of the service\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 