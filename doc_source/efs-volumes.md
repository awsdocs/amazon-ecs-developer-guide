# Amazon EFS Volumes<a name="efs-volumes"></a>


|  | 
| --- |
| Using the efsVolumeConfiguration task definition parameter remains in preview and is a Beta Service as defined by and subject to the Beta Service Participation Service Terms located at [https://aws\.amazon\.com/service\-terms](https://aws.amazon.com/service-terms) \("Beta Terms"\)\. These Beta Terms apply to your participation in this preview of the efsVolumeConfiguration task definition parameter\. | 

Amazon Elastic File System \(Amazon EFS\) provides simple, scalable file storage for use with Amazon EC2 instances\. With Amazon EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files\. Your applications can have the storage they need, when they need it\.

You can use Amazon EFS file systems with Amazon ECS to export file system data across your fleet of container instances\. That way, your tasks have access to the same persistent storage, no matter the instance on which they land\. However, you must configure your container instance AMI to mount the Amazon EFS file system before the Docker daemon starts\. Also, your task definitions must reference volume mounts on the container instance to use the file system\. The following sections help you get started using Amazon EFS with Amazon ECS\.

For a tutorial, see [Tutorial: Using Amazon EFS File Systems with Amazon ECS](using_efs.md)\.

## Amazon EFS Volume Considerations<a name="efs-volume-considerations"></a>

The following should be considered when using Amazon EFS volumes:
+ Amazon EFS volumes are only supported when using the EC2 launch type\.

## Specifying an Amazon EFS File System in your Task Definition<a name="specify-efs-config"></a>

In order to use Amazon EFS file system volumes for your containers, you must specify the volume and mount point configurations in your task definition\. The following task definition JSON snippet shows the syntax for the `volumes` and `mountPoints` objects for a container:

```
{
    "containerDefinitions": [
        {
            "name": "container-using-efs",
            "image": "amazonlinux:2",
            "entryPoint": [
                "sh",
                "-c"
            ],
            "command": [
                "ls -la /mount/efs"
            ],
            "mountPoints": [
                {
                    "sourceVolume": "myEfsVolume",
                    "containerPath": "/mount/efs",
                    "readOnly": true
                }
            ]
        }
    ],
    "volumes": [
        {
            "name": "myEfsVolume",
            "efsVolumeConfiguration": {
                "fileSystemId": "fs-1234",
                "rootDirectory": "/path/to/my/data"
            }
        }
    ]
}
```

`efsVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when using Amazon EFS volumes\. Amazon EFS volumes are only supported when using the EC2 launch type\.    
`fileSystemId`  
Type: String  
Required: Yes  
The Amazon EFS file system ID to use\.  
`rootDirectory`  
Type: String  
Required: No  
The directory within the Amazon EFS file system to mount as the root directory inside the host\.