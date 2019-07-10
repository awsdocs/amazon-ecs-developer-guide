# ecs\-cli local down<a name="cmd-ecs-cli-local-down"></a>

Stops and removes locally running containers\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-local-down-syntax"></a>

ecs\-cli local down \[\-\-task\-def\-file *filename*\] \[\-\-task\-def\-remote *task\_definition\_ARN\_family*\] \[\-\-all\] 

## Options<a name="cmd-ecs-cli-local-down-options"></a>


| Name | Description | 
| --- | --- | 
|  `--task-def-file filename`  |  Stop and remove all running containers matching the task definition filename\. If both `--task-def-file` and `--task-def-remote` are omitted, the ECS CLI defaults to `task-definition.json`\. Type: string Required: No  | 
|  `--task-def-remote value`  |  Stops and remove all running containers matching the specified task definition Amazon Resource Name \(ARN\) or family:revision\. If you specify a task definition family without a revision, the latest revision is used\. Type: string Required: No  | 
|  `--all`  |  Stops and removes all locally running containers\. Type: string Required: No  | 

## Examples<a name="cmd-ecs-cli-local-down-examples"></a>

### Stop a locally running container<a name="cmd-ecs-cli-local-down-example-1"></a>

This example stops a locally running container that is using the `hello-world.json` task definition file\.

```
ecs-cli local down --task-def-file hello-world.json
```

Output:

```
INFO[0000] Stop and remove 1 container(s)               
INFO[0011] Stopped container with id 9df4c584d905       
INFO[0011] Removed container with id 9df4c584d905       
INFO[0011] The network ecs-local-network has no more running tasks 
INFO[0012] Stopped container with name amazon-ecs-local-container-endpoints 
INFO[0012] Removed container with name amazon-ecs-local-container-endpoints 
INFO[0012] Removed network with name ecs-local-network
```

### Stop all locally running containers<a name="cmd-ecs-cli-local-down-example-1"></a>

This example stops all locally running containers\.

```
ecs-cli local down --all
```