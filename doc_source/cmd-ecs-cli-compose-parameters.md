# Using Docker Compose File Syntax<a name="cmd-ecs-cli-compose-parameters"></a>

The `ecs-cli compose` and `ecs-cli compose service` commands allow you to create task definitions and manage your Amazon ECS tasks and services using Docker compose files\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md) and [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.

At this time, the latest version of the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. Only major versions of the Docker compose file syntax are supported\. Consequently, the version specified in the compose file must be the string `"1"`, `"1.0"`, `"2"`, `"2.0"`, `"3"`, or `"3.0"`\.

By default, the Amazon ECS CLI commands look for a Docker compose file in the current directory, named `docker-compose.yml`\. However, you can also specify a different file name or path to a compose file with the `--file` option\. This is especially useful for managing tasks and services from multiple compose files at a time with the Amazon ECS CLI\.

The following parameters are supported in compose files for the Amazon ECS CLI:
+ `cap_add` \(Not valid for tasks using the Fargate launch type\)
+ `cap_drop` \(Not valid for tasks using the Fargate launch type\)
+ `command`
+ `cpu_shares`
**Note**  
If you are using the Compose version 3 format, `cpu_shares` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `devices` \(Not valid for tasks using the Fargate launch type\)
+ `dns`
+ `dns_search`
+ `entrypoint`
+ `environment`: If an environment variable value is not specified in the compose file, but it exists in the shell environment, the shell environment variable value is passed to the task definition that is created for any associated tasks or services\.
**Important**  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.
+ `env_file`
**Important**  
We do not recommend using plaintext environment variables for sensitive information, such as credential data\.
+ `extends` \(Compose file version 1 and 2 only\)
+ `extra_hosts`
+ `healthcheck` \(Compose file version 3 only\)
**Note**  
The `start_period` field is not supported using the compose file\. To specify a `start_period`, use the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `hostname`
+ `image`
+ `labels`
+ `links` \(Not valid for tasks using the Fargate launch type\)
+ `log_driver` \(Compose file version 1 only\)
+ `log_opt` \(Compose file version 1 only\)
+ `logging` \(Compose file version 2 and 3\)
  + `driver`
  + `options`
+ `mem_limit` \(in bytes\)
**Note**  
If you are using the Compose version 3 format, `mem_limit` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `mem_reservation` \(in bytes\)
**Note**  
If you are using the Compose version 3 format, `mem_reservation` should be specified in the `ecs-params.yml`\. file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `ports`
+ `privileged` \(Not valid for tasks using the Fargate launch type\)
+ `read_only`
+ `security_opt`
+ `shm_size` \(Compose file version 1 and 2 only and not valid for tasks using the Fargate launch type\)
+ `tmpfs` \(Not valid for tasks using the Fargate launch type\)
+ `ulimits`
+ `user`
+ `volumes`
+ `volumes_from` \(Compose file version 1 and 2 only\)
+ `working_dir`

**Important**  
The `build` directive is not supported at this time\.

For more information about Docker compose file syntax, see the [Compose file reference](https://docs.docker.com/compose/compose-file/#/compose-file-reference) in the Docker documentation\.