# ecs\-cli compose service scale<a name="cmd-ecs-cli-compose-service-scale"></a>

Scales the desired count of the service to the specified count\.

## Syntax<a name="cmd-ecs-cli-compose-service-scale-syntax"></a>

ecs\-cli compose service scale \[\-\-deployment\-max\-percent *n*\] \[\-\-deployment\-min\-healthy\-percent *n*\] \[\-\-timeout *value*\] *n* \[\-\-help\]

## Options<a name="cmd-ecs-cli-compose-service-scale-options"></a>


| Name | Description | 
| --- | --- | 
|  `--deployment-max-percent`  |  Specifies the upper limit \(as a percentage of the service's `desiredCount`\) of the number of running tasks that can be running in a service during a deployment\. For more information, see [maximumPercent](service_definition_parameters.md#maximumPercent)\. Default value: `200` Required: No  | 
|  `--deployment-min-healthy-percent`  |  Specifies the lower limit \(as a percentage of the service's desiredCount\) of the number of running tasks that must remain running and healthy in a service during a deployment\. For more information, see [minimumHealthyPercent](service_definition_parameters.md#minimumHealthyPercent)\. Default value: `100` Required: No  | 
|  `--timeout value`  |  Specifies the timeout value, in minutes \(decimals supported\), to wait for the running task count to change\. If the running task count has not changed for the specified period of time, the Amazon ECS CLI times out and returns an error\. Setting the timeout to `0` causes the command to return without checking for success\. The default timeout value is `5` \(minutes\)\. Default value: `5` Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-compose-service-scale-examples"></a>

### Example 1<a name="cmd-ecs-cli-compose-service-scale-example-1"></a>

This example scales the service created by the `hello-world` project to a desired count of 2\.

```
ecs-cli compose --project-name hello-world --file hello-world.yml service scale 2
```

Output:

```
INFO[0000] Updated ECS service successfully              desiredCount=2 serviceName=ecscompose-service-hello-world
INFO[0000] Service status                                desiredCount=2 runningCount=1 serviceName=ecscompose-service-hello-world
INFO[0030] (service ecscompose-service-hello-world) has started 1 tasks: (task 80602da8-442c-48ea-a8a9-80328c302b89).  timestamp=2017-08-18 21:17:44 +0000 UTC
INFO[0075] Service status                                desiredCount=2 runningCount=2 serviceName=ecscompose-service-hello-world
INFO[0075] ECS Service has reached a stable state        desiredCount=2 runningCount=2 serviceName=ecscompose-service-hello-world
```