# ecs\-cli compose service ps, list<a name="cmd-ecs-cli-compose-service-ps"></a>

Lists all the containers in your cluster that belong to the service created with the compose project\.

## Syntax<a name="cmd-ecs-cli-compose-service-ps-syntax"></a>

ecs\-cli compose service ps\|list \[\-\-desired\-status *status*\] \[\-\-help\]

## Options<a name="cmd-ecs-cli-compose-service-ps-options"></a>


| Name | Description | 
| --- | --- | 
|  `--desired-status status`  |  The container desired status to filter the container list results with\. Required: No Valid values: `RUNNING` \| `STOPPED`  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 