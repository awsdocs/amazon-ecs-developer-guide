# Using Docker Compose File Syntax<a name="cmd-ecs-cli-compose-parameters"></a>

The `ecs-cli compose` and `ecs-cli compose service` commands allow you to create task definitions and manage your Amazon ECS tasks and services using Docker Compose files\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md) and [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.

At this time, the latest version of the Amazon ECS CLI only supports the major versions of [Docker Compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. The version specified in the compose file must be the string `"1"`, `"1.0"`, `"2"`, `"2.0"`, `"3"`, or `"3.0"`\. Docker Compose minor versions are not supported\.

By default, the Amazon ECS CLI commands look for a Docker Compose file in the current directory, named `docker-compose.yml`\. However, you can also specify a different file name or path to a Compose file with the `--file` option\. This is especially useful for managing tasks and services from multiple Compose files at a time with the Amazon ECS CLI\.

The following parameters are supported in Compose files for the Amazon ECS CLI:
+ `cap_add` \(not valid for tasks using the Fargate launch type\)
+ `cap_drop` \(not valid for tasks using the Fargate launch type\)
+ `command`
+ `cpu_shares`
**Note**  
If you're using the Compose version 3\.0 format, `cpu_shares` should be specified in the `ecs-params.yml` file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `devices` \(not valid for tasks using the Fargate launch type\)
+ `dns`
+ `dns_search`
+ `entrypoint`
+ `environment`: If an environment variable value isn't specified in the Compose file, but it exists in the shell environment, the shell environment variable value is passed to the task definition that is created for any associated tasks or services\.
**Important**  
We don't recommend using plaintext environment variables for sensitive information, such as credential data\.
+ `env_file`
**Important**  
We don't recommend using plaintext environment variables for sensitive information, such as credential data\.
+ `extends` \(Compose file version 1\.0 and 2 only\)
+ `extra_hosts`
+ `healthcheck` \(Compose file version 3\.0 only\)
**Note**  
The `start_period` field isn't supported using the Compose file\. To specify a `start_period`, use the `ecs-params.yml` file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `hostname`
+ `image`
+ `labels`
+ `links` \(not valid for tasks using the Fargate launch type\)
+ `log_driver` \(Compose file version 1\.0 only\)
+ `log_opt` \(Compose file version 1\.0 only\)
+ `logging` \(Compose file version 2\.0 and 3\.0\)
  + `driver`
  + `options`
+ `mem_limit` \(in bytes\)
**Note**  
If you're using the Compose version 3\.0 format, `mem_limit` should be specified in the `ecs-params.yml` file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `mem_reservation` \(in bytes\)
**Note**  
If you're using the Compose version 3\.0 format, `mem_reservation` should be specified in the `ecs-params.yml` file\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose-ecsparams.md)\.
+ `ports`
+ `privileged` \(not valid for tasks using the Fargate launch type\)
+ `read_only`
+ `security_opt`
+ `shm_size` \(Compose file version 1\.0 and 2 only and not valid for tasks using the Fargate launch type\)
+ `tmpfs` \(not valid for tasks using the Fargate launch type\)
+ `tty`
+ `ulimits`
+ `user`
+ `volumes`
+ `volumes_from` \(Compose file version 1\.0 and 2 only\)
+ `working_dir`

**Important**  
The `build` directive isn't supported at this time\.

For more information about Docker Compose file syntax, see the [Compose file reference](https://docs.docker.com/compose/compose-file/#/compose-file-reference) in the Docker documentation\.