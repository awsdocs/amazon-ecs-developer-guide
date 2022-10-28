# Using video transcoding on Amazon ECS<a name="ecs-vt1"></a>

To use video transcoding workloads on Amazon ECS, register [Amazon EC2 VT1](http://aws.amazon.com/ec2/instance-types/vt1/) instances\. After you registered these instances, you can run live and pre\-rendered video transcoding workloads as tasks on Amazon ECS\. Amazon EC2 VT1 instances use Xilinx U30 media transcoding cards to accelerate live and pre\-rendered video transcoding workloads\.

**Note**  
For instructions on how to run video transcoding workloads in containers other than Amazon ECS, see the [Xilinx documentation](https://xilinx.github.io/video-sdk/v1.5/container_setup.html#working-with-docker-vt1)\.

## Considerations<a name="ecs-vt1-considerations"></a>

Before you begin deploying VT1 on Amazon ECS, consider the following:
+ Your clusters can contain a mix of VT1 and non\-VT1 instances\.
+ You need a Linux application that uses Xilinx U30 media transcoding cards with accelerated AVC \(H\.264\) and HEVC \(H\.265\) codecs\.
**Important**  
Applications that use other codecs might not have improved performance on VT1 instances\.
+ Only one transcoding task can run on a U30 card\. Each card has two devices that are associated with it\. You can run as many transcoding tasks as there are cards for each of your VT1 instance\.
+ When creating a service or running a standalone task, you can use instance type attributes when configuring task placement constraints\. This ensures that the task is launched on the container instance that you specify\. Doing so helps ensure that you use your resources effectively and that your tasks for video transcoding workloads are on your VT1 instances\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  In the following example, a task is run on a `vt1.3xlarge` instance on your `default` cluster\.

  ```
  aws ecs run-task \
       --cluster default \
       --task-definition vt1-3xlarge-xffmpeg-processor \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == vt1.3xlarge"
  ```
+ You configure a container to use the specific U30 card available on the host container instance\. You can do this by using the `linuxParameters` parameter and specifying the device details\. For more information, see [Task definition requirements](#ecs-vt1-requirements)\.

## Using a VT1 AMI<a name="ecs-vt1-ami"></a>

You have two options for running an AMI on Amazon EC2 for Amazon ECS container instances\. The first option is to use the Xilinx official AMI on the AWS Marketplace\. The second option is to build your own AMI from the sample repository\.
+ [Xilinx offers AMIs on the AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-phvk6d4mq3hh6)\.
+ Amazon ECS provides a sample repository that you can use to build an AMI for video transcoding workloads\. This AMI comes with Xilinx U30 drivers\. You can find the repository that contains Packer scripts on [GitHub](https://github.com/aws-samples/aws-vt-baseami-pipeline)\. For more information about Packer, see the [Packer documentation](https://www.packer.io/docs)\.

## Task definition requirements<a name="ecs-vt1-requirements"></a>

To run video transcoding containers on Amazon ECS, your task definition must contain a video transcoding application that uses the accelerated H\.264/AVC and H\.265/HEVC codecs\. You can build a container image by following the steps on the [Xilinx GitHub](https://xilinx.github.io/video-sdk/v1.5/container_setup.html#creating-a-docker-image-for-vt1-usage)\.

The task definition must be specific to the instance type\. The instance types are 3xlarge, 6xlarge, and 24xlarge\. You must configure a container to use specific Xilinx U30 devices that are available on the host container instance\. You can do so using the `linuxParameters` parameter\. The following table details the cards and device SoCs that are specific to each instance type\.


| Instance Type | vCPUs | RAM \(GiB\) | U30 accelerator cards | Addressable XCU30 SoC devices | Device Paths | 
| --- | --- | --- | --- | --- | --- | 
| vt1\.3xlarge | 12 | 24 | 1 | 2 | /dev/dri/renderD128,/dev/dri/renderD129 | 
| vt1\.6xlarge | 24 | 48 | 2 | 4 | /dev/dri/renderD128,/dev/dri/renderD129,/dev/dri/renderD130,/dev/dri/renderD131 | 
| vt1\.24xlarge | 96 | 182 | 8 | 16 | /dev/dri/renderD128,/dev/dri/renderD129,/dev/dri/renderD130,/dev/dri/renderD131,/dev/dri/renderD132,/dev/dri/renderD133,/dev/dri/renderD134,/dev/dri/renderD135,/dev/dri/renderD136,/dev/dri/renderD137,/dev/dri/renderD138,/dev/dri/renderD139,/dev/dri/renderD140,/dev/dri/renderD141,/dev/dri/renderD142,/dev/dri/renderD143 | 

**Important**  
If the task definition lists devices that the EC2 instance doesn't have, the task fails to run\. When the task fails, the following error message appears in the `stoppedReason`: `CannotStartContainerError: Error response from daemon: error gathering device information while adding custom device "/dev/dri/renderD130": no such file or directory`\.

In the following example, the syntax that's used for a task definition of a Linux container on Amazon EC2 is provided\. This task definition is for container images that are built following the procedure that's provided in the [Xilinx documentation](https://xilinx.github.io/video-sdk/v1.5/container_setup.html#creating-a-docker-image-for-vt1-usage)\. If you use this example, replace `image` with your own image, and copy your video files into the instance in the `/home/ec2-user` directory\.

------
#### [ vt1\.3xlarge ]

1. Create a text file that's named `vt1-3xlarge-ffmpeg-linux.json` with the following content\.

   ```
   {
     "family": "vt1-3xlarge-xffmpeg-processor",
     "requiresCompatibilities": ["EC2"],
     "placementConstraints": [
           {
               "type": "memberOf",
               "expression": "attribute:ecs.os-type == linux"
           },
           {
               "type": "memberOf",
               "expression": "attribute:ecs.instance-type == vt1.3xlarge"
           }
       ],
     "containerDefinitions": [
       {
         "entryPoint": [
           "/bin/bash",
           "-c"
         ],
         "command": [
           "/video/ecs_ffmpeg_wrapper.sh"
         ],
         "linuxParameters": {
           "devices": [
             {
               "containerPath": "/dev/dri/renderD128",
               "hostPath": "/dev/dri/renderD128",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD129",
               "hostPath": "/dev/dri/renderD129",
               "permissions": [
                   "read",
                   "write"
               ]
             }
           ]
         },
         "mountPoints": [
           {
             "containerPath": "/video",
             "sourceVolume": "video_file"
           }
         ],
         "cpu": 0,
         "memory": 12000,
         "image": "0123456789012.dkr.ecr.us-west-2.amazonaws.com/aws/xilinx-xffmpeg",
         "essential": true,
         "name": "xilinix-xffmpeg"
       }
     ],
     "volumes": [
       {
         "name": "video_file",
         "host": {
           "sourcePath": "/home/ec2-user"
         }
       }
     ]
   }
   ```

1. Register the task definition\.

   ```
   aws ecs register-task-definition --family vt1-3xlarge-xffmpeg-processor --cli-input-json file://vt1-3xlarge-xffmpeg-linux.json --region us-east-1
   ```

------
#### [ vt1\.6xlarge ]

1. Create a text file that's named `vt1-6xlarge-ffmpeg-linux.json` with the following content\.

   ```
   {
     "family": "vt1-6xlarge-xffmpeg-processor",
     "requiresCompatibilities": ["EC2"],
     "placementConstraints": [
           {
               "type": "memberOf",
               "expression": "attribute:ecs.os-type == linux"
           },
           {
               "type": "memberOf",
               "expression": "attribute:ecs.instance-type == vt1.6xlarge"
           }
       ],
     "containerDefinitions": [
       {
         "entryPoint": [
           "/bin/bash",
           "-c"
         ],
         "command": [
           "/video/ecs_ffmpeg_wrapper.sh"
         ],
         "linuxParameters": {
           "devices": [
             {
               "containerPath": "/dev/dri/renderD128",
               "hostPath": "/dev/dri/renderD128",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD129",
               "hostPath": "/dev/dri/renderD129",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD130",
               "hostPath": "/dev/dri/renderD130",
               "permissions": [
                   "read",
                   "write"
                ]
              },
              {
                "containerPath": "/dev/dri/renderD131",
                "hostPath": "/dev/dri/renderD131",
                "permissions": [
                    "read",
                    "write"
                ]
              }
           ]
         },
         "mountPoints": [
           {
             "containerPath": "/video",
             "sourceVolume": "video_file"
           }
         ],
         "cpu": 0,
         "memory": 12000,
         "image": "0123456789012.dkr.ecr.us-west-2.amazonaws.com/aws/xilinx-xffmpeg",
         "essential": true,
         "name": "xilinix-xffmpeg"
       }
     ],
     "volumes": [
       {
         "name": "video_file",
         "host": {
           "sourcePath": "/home/ec2-user"
         }
       }
     ]
   }
   ```

1. Register the task definition\.

   ```
   aws ecs register-task-definition --family vt1-6xlarge-xffmpeg-processor --cli-input-json file://vt1-6xlarge-xffmpeg-linux.json --region us-east-1
   ```

------
#### [ vt1\.24xlarge ]

1. Create a text file that's named `vt1-24xlarge-ffmpeg-linux.json` with the following content\.

   ```
   {
     "family": "vt1-24xlarge-xffmpeg-processor",
     "requiresCompatibilities": ["EC2"],
     "placementConstraints": [
           {
               "type": "memberOf",
               "expression": "attribute:ecs.os-type == linux"
           },
           {
               "type": "memberOf",
               "expression": "attribute:ecs.instance-type == vt1.24xlarge"
           }
       ],
     "containerDefinitions": [
       {
         "entryPoint": [
           "/bin/bash",
           "-c"
         ],
         "command": [
           "/video/ecs_ffmpeg_wrapper.sh"
         ],
         "linuxParameters": {
           "devices": [
             {
               "containerPath": "/dev/dri/renderD128",
               "hostPath": "/dev/dri/renderD128",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD129",
               "hostPath": "/dev/dri/renderD129",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD130",
               "hostPath": "/dev/dri/renderD130",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD131",
               "hostPath": "/dev/dri/renderD131",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD132",
               "hostPath": "/dev/dri/renderD132",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD133",
               "hostPath": "/dev/dri/renderD133",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD134",
               "hostPath": "/dev/dri/renderD134",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD135",
               "hostPath": "/dev/dri/renderD135",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD136",
               "hostPath": "/dev/dri/renderD136",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD137",
               "hostPath": "/dev/dri/renderD137",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD138",
               "hostPath": "/dev/dri/renderD138",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD139",
               "hostPath": "/dev/dri/renderD139",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD140",
               "hostPath": "/dev/dri/renderD140",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD141",
               "hostPath": "/dev/dri/renderD141",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD142",
               "hostPath": "/dev/dri/renderD142",
               "permissions": [
                   "read",
                   "write"
               ]
             },
             {
               "containerPath": "/dev/dri/renderD143",
               "hostPath": "/dev/dri/renderD143",
               "permissions": [
                   "read",
                   "write"
               ]
             }
           ]
         },
         "mountPoints": [
           {
             "containerPath": "/video",
             "sourceVolume": "video_file"
           }
         ],
         "cpu": 0,
         "memory": 12000,
         "image": "0123456789012.dkr.ecr.us-west-2.amazonaws.com/aws/xilinx-xffmpeg",
         "essential": true,
         "name": "xilinix-xffmpeg"
       }
     ],
     "volumes": [
       {
         "name": "video_file",
         "host": {
           "sourcePath": "/home/ec2-user"
         }
       }
     ]
   }
   ```

1. Register the task definition\.

   ```
   aws ecs register-task-definition --family vt1-24xlarge-xffmpeg-processor --cli-input-json file://vt1-24xlarge-xffmpeg-linux.json --region us-east-1
   ```

------