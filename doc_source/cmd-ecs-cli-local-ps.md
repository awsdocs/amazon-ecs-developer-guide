# ecs\-cli local ps<a name="cmd-ecs-cli-local-ps"></a>

Lists locally running containers\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-local-ps-syntax"></a>

ecs\-cli local ps \[\-\-task\-def\-file *filename*\] \[\-\-task\-def\-remote *value*\] \[\-\-all\] \[\-\-json\] 

## Options<a name="cmd-ecs-cli-local-ps-options"></a>


| Name | Description | 
| --- | --- | 
|  `--task-def-file filename`  |  Lists all locally running containers matching the task definition filename\. If both `--task-def-file` and `--task-def-remote` are omitted, the ECS CLI defaults to `task-definition.json`\. Type: string Required: No  | 
|  `--task-def-remote value`  |  Lists all running containers matching the task definition Amazon Resource Name \(ARN\) or family:revision\. If you specify a task definition family without a revision, the latest revision is used\. Type: string Required: No  | 
|  `--all`  |  Lists all locally running containers\. Type: string Required: No  | 
|  `--json`  |  Sets the output to JSON format\. Type: string Required: No  | 

## Examples<a name="cmd-ecs-cli-local-ps-examples"></a>

### List all locally running containers<a name="cmd-ecs-cli-local-ps-example-1"></a>

This example lists all locally running containers\.

```
ecs-cli local ps --all
```

Output:

```
CONTAINER ID        IMAGE               STATUS              PORTS                NAMES                     TASKDEFINITION
9df4c584d905        httpd:2.4           Up 15 seconds       0.0.0.0:80->80/tcp   /downloads_simple-app_1   /Users/brandejo/Downloads/task-definition.json
```

### List all locally running containers using a specified task definition file<a name="cmd-ecs-cli-local-ps-example-1"></a>

This example lists the locally running containers using the `hello-world.json` task definition\.

```
ecs-cli local ps --task-def-file hello-world.json
```