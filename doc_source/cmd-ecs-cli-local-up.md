# ecs\-cli local up<a name="cmd-ecs-cli-local-up"></a>

Runs containers locally from an Amazon ECS task definition\. By default this commands looks for a task definition JSON file named `task-definition.json` in the current directory\. If the task definition file does not exist, then one must be specified using one of the `--task-def` options described below\. This command also creates a local Docker Compose file as specified in the `--output` option prior to running the containers\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-local-up-syntax"></a>

ecs\-cli local up \[\-\-task\-def\-compose *filename*\] \[\-\-task\-def\-file *filename*\] \[\-\-task\-def\-remote *value*\] \[\-\-force\] \[\-\-output *output\_file*\] \[\-\-override *filename*\] 

## Options<a name="cmd-ecs-cli-local-up-options"></a>


| Name | Description | 
| --- | --- | 
|  `--task-def-compose filename`  |  Specifies the Docker Compose file to run locally\. Type: string Required: No  | 
|  `--task-def-file filename`  |  Specifies the task definition JSON file to run locally\. If one is not specified, the ECS CLI will look for a file named `task-definition.json` in the current directory\. Type: string Required: No  | 
|  `--task-def-remote value`  |  Specifies the full Amazon Resource Name \(ARN\) or family:revision of the task definition to convert to a Docker Compose file\. If you specify a task definition family without a revision, the latest revision is used\. Type: string Required: No  | 
|  `--force`  |  Overwrites any existing Docker Compose output file without prompting for confirmation\.  | 
|  `--output output_file`  |  Specifies the local filename to write the Docker Compose file to\. If one is not specified, the default is `docker-compose.local.yml`\. If the output file already exists, the CLI will prompt you with an overwrite request\. Type: string Required: No  | 
|  `--override filename`  |  Specifies the local Docker Compose override filename to use\. Type: string Required: No  | 

## Examples<a name="cmd-ecs-cli-local-up-examples"></a>

### Run containers locally from a local task definition JSON file<a name="cmd-ecs-cli-local-up-example-1"></a>

This example runs the containers locally that are defined in a local task definition file named `hello-world.json`\.

```
ecs-cli local create --task-def-file hello-world.json
```

Output:

```
INFO[0001] Successfully wrote docker-compose.ecs-local.yml 
INFO[0002] Successfully wrote docker-compose.ecs-local.override.yml 
INFO[0002] The network ecs-local-network already exists 
INFO[0002] The amazon-ecs-local-container-endpoints container already exists with ID 5976522f4cafb840e5f003a2285fc439ed1b2a89aa74634958c6a6105ca6edd1 
INFO[0002] Started container with ID 5976522f4cafb840e5f003a2285fc439ed1b2a89aa74634958c6a6105ca6edd1 
INFO[0002] Using docker-compose.ecs-local.yml, docker-compose.ecs-local.override.yml files to start containers 
Compose out: Found orphan containers (downloads_httpd_1) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up.
Creating downloads_simple-app_1 ... done
```

### Run containers locally from a remote task definition<a name="cmd-ecs-cli-local-up-example-2"></a>

This example runs the containers locally that are defined in the latest revision of an Amazon ECS task definition named `hello-world`\.

```
ecs-cli local up --task-def-remote hello-world
```

Output:

```
INFO[0000] Reading task definition from hello-world
  
INFO[0002] Successfully wrote docker-compose.ecs-local.yml 
INFO[0004] Successfully wrote docker-compose.ecs-local.override.yml 
INFO[0004] The network ecs-local-network already exists 
INFO[0005] The amazon-ecs-local-container-endpoints container already exists with ID 5976522f4cafb840e5f003a2285fc439ed1b2a89aa74634958c6a6105ca6edd1 
INFO[0005] Started container with ID 5976522f4cafb840e5f003a2285fc439ed1b2a89aa74634958c6a6105ca6edd1 
INFO[0005] Using docker-compose.ecs-local.yml, docker-compose.ecs-local.override.yml files to start containers 
Compose out: Found orphan containers (downloads_httpd_1) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up.
Creating downloads_hello-world_1 ... done
```