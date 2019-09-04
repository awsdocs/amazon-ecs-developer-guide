# ecs\-cli local create<a name="cmd-ecs-cli-local-create"></a>

Creates a Docker Compose file from an Amazon ECS task definition\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-local-create-syntax"></a>

ecs\-cli local create \[\-\-task\-def\-file *filename*\] \[\-\-task\-def\-remote *value*\] \[\-\-force\] \[\-\-output *output\_file*\] 

## Options<a name="cmd-ecs-cli-local-create-options"></a>


| Name | Description | 
| --- | --- | 
|  `--task-def-file filename`  |  Specifies the filename that contains the task definition JSON to convert to a Docker Compose file\. If one is not specified, the ECS CLI will look for a file named `task-definition.json` in the current directory\. Type: JSON Required: No  | 
|  `--task-def-remote value`  |  Specifies the full Amazon Resource Name \(ARN\) or family:revision of the task definition to convert to a Docker Compose file\. If you specify a task definition family without a revision, the latest revision is used\. Type: string Required: No  | 
|  `--force`  |  Overwrites any existing Docker Compose output file without prompting for confirmation\.  | 
|  `--output output_file`  |  Specifies the local filename to write the Docker Compose file to\. If one is not specified, the default is `docker-compose.local.yml`\. Type: string Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-local-create-examples"></a>

### Create a Docker Compose file from a local JSON file<a name="cmd-ecs-cli-local-create-example-1"></a>

This example creates a Docker Compose file from a local JSON file containing an Amazon ECS task definition\.

```
ecs-cli local create --task-def-file task-definition.json
```

Output:

```
INFO[0000] Successfully wrote docker-compose.ecs-local.yml 
INFO[0000] Successfully wrote docker-compose.ecs-local.override.yml
```

### Create a Docker Compose file from a remote task definition<a name="cmd-ecs-cli-local-create-example-2"></a>

This example creates a Docker Compose file from the latest revision of an Amazon ECS task definition named `hello-world`\.

```
ecs-cli local create --task-def-remote hello-world
```

Output:

```
INFO[0000] Successfully wrote docker-compose.ecs-local.yml 
INFO[0000] Successfully wrote docker-compose.ecs-local.override.yml
```