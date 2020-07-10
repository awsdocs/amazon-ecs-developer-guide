# Enabling Docker debug output<a name="docker-debug-mode"></a>

If you are having trouble with Docker containers or images, you can enable debug mode on your Docker daemon\. Enabling debugging provides more verbose output from the daemon and you can use this information to find out more about why your containers or images are having issues\.

Enabling Docker debug mode can be especially useful in retrieving error messages that are sent from container registries, such as Amazon ECR, and, in many circumstances, enabling debug mode is the only way to see these error messages\.

**Important**  
This procedure is written for the Amazon ECS\-optimized Amazon Linux AMI\. For other operating systems, see [Enable debugging](https://docs.docker.com/engine/admin/#enable-debugging) and [Control and configure Docker with systemd]() in the Docker documentation\.

**To enable Docker daemon debug mode on the Amazon ECS\-optimized Amazon Linux AMI**

1. Connect to your container instance\. For more information, see [Connect to your container instance](instance-connect.md)\.

1. Open the Docker options file with a text editor, such as vi\. For the Amazon ECS\-optimized Amazon Linux AMI, the Docker options file is at `/etc/sysconfig/docker`\.

1. Find the Docker options statement and add the `-D` option to the string, inside the quotes\.
**Note**  
If the Docker options statement begins with a `#`, remove that character to uncomment the statement and enable the options\.

   For the Amazon ECS\-optimized Amazon Linux AMI, the Docker options statement is called `OPTIONS`\. For example:

   ```
   # Additional startup options for the Docker daemon, for example:
   # OPTIONS="--ip-forward=true --iptables=true"
   # By default we limit the number of open files per container
   OPTIONS="-D --default-ulimit nofile=1024:4096"
   ```

1. Save the file and exit your text editor\.

1. Restart the Docker daemon\.

   ```
   sudo service docker restart
   ```

   Output:

   ```
   Stopping docker:                                          [  OK  ]
   Starting docker:	.                                  [  OK  ]
   ```

1. Restart the Amazon ECS agent\.

   ```
   sudo service ecs restart
   ```

Your Docker logs should now show more verbose output\. For example:

```
time="2015-12-30T21:48:21.907640838Z" level=debug msg="Unexpected response from server: \"{\\\"errors\\\":[{\\\"code\\\":\\\"DENIED\\\",\\\"message\\\":\\\"User: arn:aws:sts::1111:assumed-role/ecrReadOnly/i-abcdefg is not authorized to perform: ecr:InitiateLayerUpload on resource: arn:aws:ecr:us-east-1:1111:repository/nginx_test\\\"}]}\\n\" http.Header{\"Connection\":[]string{\"keep-alive\"}, \"Content-Type\":[]string{\"application/json; charset=utf-8\"}, \"Date\":[]string{\"Wed, 30 Dec 2015 21:48:21 GMT\"}, \"Docker-Distribution-Api-Version\":[]string{\"registry/2.0\"}, \"Content-Length\":[]string{\"235\"}}"
```