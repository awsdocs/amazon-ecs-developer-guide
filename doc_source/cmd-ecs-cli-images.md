# ecs\-cli images<a name="cmd-ecs-cli-images"></a>

## Description<a name="cmd-ecs-cli-images-description"></a>

List images in an Amazon ECR registry or repository\.

## Syntax<a name="cmd-ecs-cli-images-syntax"></a>

**ecs\-cli images \[\-\-registry\-id *registry\_id*\] \[\-\-tagged|\-\-untagged\] \[\-\-region *region*\] \[*ECR\_REPOSITORY*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-images-options"></a>


| Name | Description | 
| --- | --- | 
|  `--registry-id registry_id`  |  Specifies the ECR registry with which to list images\. By default, images are listed for the current AWS account\. Required: No  | 
|  `--tagged`  |  Filters the result to show only tagged images\. Required: No  | 
|  `--untagged`  |  Filters the result to show only untagged images\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-images-examples"></a>

### Example 1<a name="cmd-ecs-cli-images-example-1"></a>

This example lists all of the images in an ECR registry\.

```
ecs-cli images
```

Output:

```
REPOSITORY NAME          TAG                 IMAGE DIGEST                                                              PUSHED AT           SIZE
rkt                      latest              sha256:404758ad8af94347fc8582fc8e30b6284f2b0751de29b2e755da212f80232fac   3 months ago        203 MB
foobuntu                 latest              sha256:6b079ae764a6affcb632231349d4a5e1b084bece8c46883c099863ee2aeb5cf8   4 days ago          51.7 MB
ubuntu                   xenial              sha256:6b079ae764a6affcb632231349d4a5e1b084bece8c46883c099863ee2aeb5cf8   4 days ago          51.7 MB
ubuntu                   latest              sha256:6b079ae764a6affcb632231349d4a5e1b084bece8c46883c099863ee2aeb5cf8   4 days ago          51.7 MB
ubuntu                   <none>              sha256:512e30a26d9fa3648dbccb9e78e9bab636e6022e2d80bd73c99177b21a0d3982   19 minutes ago      268 MB
ubuntu                   trusty              sha256:bd6d24e8fa3f5822146b2c94247976b87e6564195c3c180b67833e6ea699f7c2   18 minutes ago      67.2 MB
ubuntu                   precise             sha256:b38267a51fb4460699bc2bcdbb53d42fec697bb4e4f9a819df3e762cec393b2a   17 minutes ago      40.1 MB
amazon-ecs-sample        latest              sha256:bf04071a8edecc309f4d109ae36f24a5c272a115b6f7e636f77940059024d71c   2 weeks ago         105 MB
golang                   latest              sha256:137b22efee2df470b0cd28ebfc1ae583be0baf09334a5a882096193577d983ab   4 days ago          266 MB
amazonlinux              latest              sha256:a59d563b5139deee8cb108bfb97bf3e9021b8ccea6dec8ff49733230cb2f0eca   4 days ago          98.8 MB
awsbatch/fetch_and_run   latest              sha256:543800007416d0ccff4f63643bb18eeff4b874ea772128efcdc231ff456a37fc   6 weeks ago         116 MB
```

### Example 2<a name="cmd-ecs-cli-images-example-2"></a>

This example lists all of the images in a specific ECR repository\.

```
ecs-cli images ubuntu
```

Output:

```
REPOSITORY NAME     TAG                 IMAGE DIGEST                                                              PUSHED AT           SIZE
ubuntu              xenial              sha256:6b079ae764a6affcb632231349d4a5e1b084bece8c46883c099863ee2aeb5cf8   4 days ago          51.7 MB
ubuntu              latest              sha256:6b079ae764a6affcb632231349d4a5e1b084bece8c46883c099863ee2aeb5cf8   4 days ago          51.7 MB
ubuntu              <none>              sha256:512e30a26d9fa3648dbccb9e78e9bab636e6022e2d80bd73c99177b21a0d3982   20 minutes ago      268 MB
ubuntu              trusty              sha256:bd6d24e8fa3f5822146b2c94247976b87e6564195c3c180b67833e6ea699f7c2   19 minutes ago      67.2 MB
ubuntu              precise             sha256:b38267a51fb4460699bc2bcdbb53d42fec697bb4e4f9a819df3e762cec393b2a   18 minutes ago      40.1 MB
```

### Example 3<a name="cmd-ecs-cli-images-example-3"></a>

This example lists all of the untagged images in an ECR registry\.

```
ecs-cli images --untagged
```

Output:

```
REPOSITORY NAME     TAG                 IMAGE DIGEST                                                              PUSHED AT           SIZE
ubuntu              <none>              sha256:512e30a26d9fa3648dbccb9e78e9bab636e6022e2d80bd73c99177b21a0d3982   24 minutes ago      268 MB
```