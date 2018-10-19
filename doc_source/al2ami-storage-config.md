# Storage Configuration<a name="al2ami-storage-config"></a>

By default, the Amazon ECS\-optimized Amazon Linux 2 AMI ships with a single 30 GiB root volume, with a base snapshot size of 2 GiB\. You can modify the 30 GiB root volume size at launch time to increase or decrease the available storage on your container instance\. This storage is used for the operating system and for Docker images and metadata\.

The default filesystem for the Amazon ECS\-optimized Amazon Linux 2 AMI is `ext4`, and Docker uses the OverlayFS storage driver\. For more information, see [Use the OverlayFS storage driver](https://docs.docker.com/storage/storagedriver/overlayfs-driver/) in the Docker documentation\.