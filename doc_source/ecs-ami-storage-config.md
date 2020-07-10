# AMI storage configuration<a name="ecs-ami-storage-config"></a>

The following describes the storage configuration for each of the Amazon ECS\-optimized AMIs\.

**Topics**
+ [Amazon Linux 2 storage configuration](#al2-ami-storage-config)
+ [Amazon ECS\-optimized Amazon Linux AMI storage configuration](#al1-ami-storage-config)

## Amazon Linux 2 storage configuration<a name="al2-ami-storage-config"></a>

By default, the Amazon Linux 2\-based Amazon ECS\-optimized AMIs \(Amazon ECS\-optimized Amazon Linux 2 AMI, Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI, and Amazon ECS GPU\-optimized AMI\) ship with a single 30\-GiB root volume\. You can modify the 30\-GiB root volume size at launch time to increase the available storage on your container instance\. This storage is used for the operating system and for Docker images and metadata\.

The default filesystem for the Amazon ECS\-optimized Amazon Linux 2 AMI is `ext4`, and Docker uses the `overlay2` storage driver\. For more information, see [Use the OverlayFS storage driver](https://docs.docker.com/storage/storagedriver/overlayfs-driver/) in the Docker documentation\.

## Amazon ECS\-optimized Amazon Linux AMI storage configuration<a name="al1-ami-storage-config"></a>

By default, the Amazon ECS\-optimized Amazon Linux AMI ships with 30 GiB of total storage\. You can modify this value at launch time to increase the available storage on your container instance\. This storage is used for the operating system and for Docker images and metadata\. The sections below describe the storage configuration of the Amazon ECS\-optimized Amazon Linux AMI, based on the AMI version\.

### Version 2015\.09\.d and later<a name="ecs-AMI-LVM"></a>

Amazon ECS\-optimized Amazon Linux AMIs from version `2015.09.d` and later launch with an 8\-GiB volume for the operating system that is attached at `/dev/xvda` and mounted as the root of the file system\. There is an additional 22\-GiB volume that is attached at `/dev/xvdcz` that Docker uses for image and metadata storage\. The volume is configured as a Logical Volume Management \(LVM\) device and it is accessed directly by Docker via the `devicemapper` backend\. Because the volume is not mounted, you cannot use standard storage information commands \(such as df \-h\) to determine the available storage\. However, you can use LVM commands and docker info to find the available storage by following the procedure below\. For more information, see the [LVM HOWTO](http://tldp.org/HOWTO/LVM-HOWTO/) in The Linux Documentation Project\.

**Note**  
You can increase these default volume sizes by changing the block device mapping settings for your instances when you launch them; however, you cannot specify a smaller volume size than the default\. For more information, see [Block Device Mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) in the *Amazon EC2 User Guide for Linux Instances*\.

The docker\-storage\-setup utility configures the LVM volume group and logical volume for Docker when the instance launches\. By default, docker\-storage\-setup creates a volume group called `docker`, adds `/dev/xvdcz` as a physical volume to that group\. It then creates a logical volume called `docker-pool` that uses 99% of the available storage in the volume group\. The remaining 1% of the available storage is reserved for metadata\.

**Note**  
Earlier Amazon ECS\-optimized Amazon Linux AMI versions \(`2015.09.d` to `2016.03.a`\) create a logical volume that uses 40% of the available storage in the volume group\. When the logical volume becomes 60% full, the logical volume is increased in size by 20%\.

**To determine the available storage for Docker**
+ You can use the LVM commands, vgs and lvs, or the docker info command to view available storage for Docker\.
**Note**  
The LVM command output displays storage values in GiB \(2^30 bytes\), and docker info displays storage values in GB \(10^9 bytes\)\.

  1. You can view the available storage in the volume group with the vgs command\. This command shows the total size of the volume group and the available space in the volume group that can be used to grow the logical volume\. The example below shows a 22\-GiB volume with 204 MiB of free space\.

     ```
     [ec2-user ~]$ sudo vgs
     ```

     Output:

     ```
       VG     #PV #LV #SN Attr   VSize  VFree
       docker   1   1   0 wz--n- 22.00g 204.00m
     ```

  1. You can view the available space in the logical volume with the lvs command\. The example below shows a logical volume that is 21\.75 GiB in size, and it is 7\.63% full\. This logical volume can grow until there is no more free space in the volume group\.

     ```
     [ec2-user@ ~]$ sudo lvs
     ```

     Output:

     ```
       LV          VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
       docker-pool docker twi-aot--- 21.75g             7.63   4.96
     ```

  1. The docker info command also provides information about how much data space it is using, and how much data space is available\. However, its available space value is based on the logical volume size that it is using\.
**Note**  
Because docker info displays storage values as GB \(10^9 bytes\), instead of GiB \(2^30 bytes\), the values displayed here look larger for the same amount of storage displayed with lvs\. However, the values are equal \(23\.35 GB = 21\.75 GiB\)\.

     ```
     [ec2-user ~]$ docker info | grep "Data Space"
     ```

     Output:

     ```
      Data Space Used: 1.782 GB
      Data Space Total: 23.35 GB
      Data Space Available: 21.57 GB
     ```

**To extend the Docker logical volume**

The easiest way to add storage to your container instances is to terminate the existing instances and launch new ones with larger data storage volumes\. However, if you are unable to do this, you can add storage to the volume group that Docker uses and extend its logical volume by following these steps\.
**Note**  
If your container instance storage is filling up too quickly, there are a few actions that you can take to reduce this effect:  
\(Amazon ECS container agent 1\.8\.0 and later\) Reduce the amount of time that stopped or exited containers remain on your container instances\. The `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` agent configuration variable sets the time duration to wait from when a task is stopped until the Docker container is removed \(by default, this value is 3 hours\)\. This removes the Docker container data\. If this value is set too low, you may not be able to inspect your stopped containers or view the logs before they are removed\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
Remove non\-running containers and unused images from your container instances\. You can use the following example commands to manually remove stopped containers and unused images\. Deleted containers cannot be inspected later, and deleted images must be pulled again before starting new containers from them\.  
To remove non\-running containers, execute the following command on your container instance:  

  ```
  $ docker rm $(docker ps -aq)
  ```
To remove unused images, execute the following command on your container instance:  

  ```
  $ docker rmi $(docker images -q)
  ```
Remove unused data blocks within containers\. You can use the following command to run fstrim on any running container and discard any data blocks that are unused by the container file system\.  

  ```
  $ sudo sh -c "docker ps -q | xargs docker inspect --format='{{ .State.Pid }}' | xargs -IZ fstrim /proc/Z/root/"
  ```

1. Create a new Amazon EBS volume in the same Availability Zone as your container instance\. For more information, see [Creating an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Attach the volume to your container instance\. The default location for the Docker data volume is `/dev/xvdcz`\. For consistency, attach additional volumes in reverse alphabetical order from that device name \(for example, `/dev/xvdcy`\)\. For more information, see [Attaching an Amazon EBS Volume to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

1. Connect to your container instance using SSH\. For more information, see [Connect to your container instance](instance-connect.md)\.

1. Check the size of your `docker-pool` logical volume\. The example below shows a logical volume of 409\.19 GiB\.

   ```
   [ec2-user ~]$ sudo lvs
   ```

   Output:

   ```
     LV          VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
     docker-pool docker twi-aot--- 409.19g             0.16   0.08
   ```

1. Check the current available space in your volume group\. The example below shows 612\.75 GiB in the `VFree` column\.

   ```
   [ec2-user ~]$ sudo vgs
   ```

   Output:

   ```
     VG     #PV #LV #SN Attr   VSize    VFree
     docker   1   1   0 wz--n- 1024.00g 612.75g
   ```

1. Add the new volume to the `docker` volume group, substituting the device name to which you attached the new volume\. In this example, a 1\-TiB volume was previously added and attached to `/dev/xvdcy`\.

   ```
   [ec2-user ~]$ sudo vgextend docker /dev/xvdcy
     Physical volume "/dev/sdcy" successfully created
     Volume group "docker" successfully extended
   ```

1. Verify that your volume group size has increased with the vgs command\. The `VFree` column should show the increased storage size\. The example below now has 1\.6 TiB in the `VFree` column, which is 1 TiB larger than it was previously\. Your `VFree` column should be the sum of the original `VFree` value and the size of the volume you attached\.

   ```
   [ec2-user ~]$ sudo vgs
   ```

   Output:

   ```
     VG     #PV #LV #SN Attr   VSize VFree
     docker   2   1   0 wz--n- 2.00t 1.60t
   ```

1. Extend the `docker-pool` logical volume with the size of the volume you added earlier\. The command below adds 1024 GiB to the logical volume, which is entered as `1024G`\.

   ```
   [ec2-user ~]$ sudo lvextend -L+1024G /dev/docker/docker-pool
   ```

   Output:

   ```
     Size of logical volume docker/docker-pool_tdata changed from 409.19 GiB (104752 extents) to 1.40 TiB (366896 extents).
     Logical volume docker-pool successfully resized
   ```

1. Verify that your logical volume has increased in size\.

   ```
   [ec2-user ~]$ sudo lvs
   ```

   Output:

   ```
     LV          VG     Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
     docker-pool docker twi-aot--- 1.40t             0.04   0.12
   ```

1. \(Optional\) Verify that docker info also recognizes the added storage space\.
**Note**  
Because docker info displays storage values as GB \(10^9 bytes\), instead of GiB \(2^30 bytes\), the values displayed here look larger for the same amount of storage displayed with lvs\. However, the values are equal \(1\.539 TB =1\.40 TiB\)\.

   ```
   [ec2-user ~]$ docker info | grep "Data Space"
   ```

   Output:

   ```
    Data Space Used: 109.6 MB
    Data Space Total: 1.539 TB
    Data Space Available: 1.539 TB
   ```

### Version 2015\.09\.c and earlier<a name="ecs-AMI-pre-LVM"></a>

Amazon ECS\-optimized Amazon Linux AMIs from version `2015.09.c` and earlier launch with a single 30\-GiB volume that is attached at `/dev/xvda` and mounted as the root of the file system\. This volume shares the operating system and all Docker images and metadata\. You can determine the available storage on your container instance with standard storage information commands \(such as df \-h\)\.

There is no practical way to add storage \(that Docker can use\) to instances launched from these AMIs without stopping them\. If you find that your container instances need more storage than the default 30 GiB, you should terminate each instance\. Then, launch another in its place with the latest Amazon ECS\-optimized Amazon Linux AMI and a large enough data storage volume\.