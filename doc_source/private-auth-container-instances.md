# Private Registry Authentication for Container Instances<a name="private-auth-container-instances"></a>

The Amazon ECS container agent can authenticate with private registries, including Docker Hub, using basic authentication\. When you enable private registry authentication, you can use private Docker images in your task definitions\. This feature is only supported by tasks using the EC2 launch type\.

Another method of enabling private registry authentication uses AWS Secrets Manager to store your private registry credentials securely and then reference them in your container definition\. This allows your tasks to use images from private repositories\. This method supports tasks using either the EC2 or Fargate launch types\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

The Amazon ECS container agent looks for two environment variables when it launches:
+ `ECS_ENGINE_AUTH_TYPE`, which specifies the type of authentication data that is being sent\.
+ `ECS_ENGINE_AUTH_DATA`, which contains the actual authentication credentials\.

Linux variants of the Amazon ECS\-optimized AMI scan the `/etc/ecs/ecs.config` file for these variables when the container instance launches, and each time the service is started \(with the sudo start ecs command\)\. AMIs that are not Amazon ECS\-optimized should store these environment variables in a file and pass them with the `--env-file path_to_env_file` option to the docker run command that starts the container agent\.

**Important**  
We do not recommend that you inject these authentication environment variables at instance launch with Amazon EC2 user data or pass them with the `--env` option to the docker run command\. These methods are not appropriate for sensitive data, such as authentication credentials\. For information about safely adding authentication credentials to your container instances, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3)\.

## Authentication Formats<a name="docker-auth-formats"></a>

There are two available formats for private registry authentication, `dockercfg` and `docker`\.

**dockercfg Authentication Format**  
The `dockercfg` format uses the authentication information stored in the configuration file that is created when you run the docker login command\. You can create this file by running docker login on your local system and entering your registry user name, password, and email address\. You can also log in to a container instance and run the command there\. Depending on your Docker version, this file is saved as either `~/.dockercfg` or `~/.docker/config.json`\.

```
cat ~/.docker/config.json
```

Output:

```
{
  "auths": {
    "https://index.docker.io/v1/": {
      "auth": "zq212MzEXAMPLE7o6T25Dk0i"
    }
  }
}
```

**Important**  
Newer versions of Docker create a configuration file as shown above with an outer `auths` object\. The Amazon ECS agent only supports `dockercfg` authentication data that is in the below format, without the `auths` object\. If you have the jq utility installed, you can extract this data with the following command: cat \~/\.docker/config\.json \| jq \.auths

```
cat ~/.docker/config.json | jq .auths
```

Output:

```
{
  "https://index.docker.io/v1/": {
    "auth": "zq212MzEXAMPLE7o6T25Dk0i",
    "email": "email@example.com"
  }
}
```

In the above example, the following environment variables should be added to the environment variable file \(`/etc/ecs/ecs.config` for the Amazon ECS\-optimized AMI\) that the Amazon ECS container agent loads at runtime\. If you are not using an Amazon ECS\-optimized AMI and you are starting the agent manually with docker run, specify the environment variable file with the `--env-file path_to_env_file` option when you start the agent\.

```
ECS_ENGINE_AUTH_TYPE=dockercfg
ECS_ENGINE_AUTH_DATA={"https://index.docker.io/v1/":{"auth":"zq212MzEXAMPLE7o6T25Dk0i","email":"email@example.com"}}
```

You can configure multiple private registries with the following syntax:

```
ECS_ENGINE_AUTH_TYPE=dockercfg
ECS_ENGINE_AUTH_DATA={"repo.example-01.com":{"auth":"zq212MzEXAMPLE7o6T25Dk0i","email":"email@example-01.com"},"repo.example-02.com":{"auth":"fQ172MzEXAMPLEoF7225DU0j","email":"email@example-02.com"}}
```

**docker Authentication Format**  
The `docker` format uses a JSON representation of the registry server that the agent should authenticate with\. It also includes the authentication parameters required by that registry \(such as user name, password, and the email address for that account\)\. For a Docker Hub account, the JSON representation looks like the following:

```
{
  "https://index.docker.io/v1/": {
    "username": "my_name",
    "password": "my_password",
    "email": "email@example.com"
  }
}
```

In this example, the following environment variables should be added to the environment variable file \(`/etc/ecs/ecs.config` for the Amazon ECS\-optimized AMI\) that the Amazon ECS container agent loads at runtime\. If you are not using an Amazon ECS\-optimized AMI, and you are starting the agent manually with docker run, specify the environment variable file with the `--env-file path_to_env_file` option when you start the agent\.

```
ECS_ENGINE_AUTH_TYPE=docker
ECS_ENGINE_AUTH_DATA={"https://index.docker.io/v1/":{"username":"my_name","password":"my_password","email":"email@example.com"}}
```

You can configure multiple private registries with the following syntax:

```
ECS_ENGINE_AUTH_TYPE=docker
ECS_ENGINE_AUTH_DATA={"repo.example-01.com":{"username":"my_name","password":"my_password","email":"email@example-01.com"},"repo.example-02.com":{"username":"another_name","password":"another_password","email":"email@example-02.com"}}
```

## Enabling Private Registries<a name="enabling-private-registry"></a>

Use the following procedure to enable private registries for your container instances\.

**To enable private registries in the Amazon ECS\-optimized AMI**

1. Log in to your container instance using SSH\.

1. Open the `/etc/ecs/ecs.config` file and add the `ECS_ENGINE_AUTH_TYPE` and `ECS_ENGINE_AUTH_DATA` values for your registry and account:

   ```
   sudo vi /etc/ecs/ecs.config
   ```

   This example authenticates a Docker Hub user account:

   ```
   ECS_ENGINE_AUTH_TYPE=docker
   ECS_ENGINE_AUTH_DATA={"https://index.docker.io/v1/":{"username":"my_name","password":"my_password","email":"email@example.com"}}
   ```

1. Check to see if your agent uses the `ECS_DATADIR` environment variable to save its state:

   ```
   docker inspect ecs-agent | grep ECS_DATADIR
   ```

   Output:

   ```
   "ECS_DATADIR=/data",
   ```
**Important**  
If the previous command does not return the `ECS_DATADIR` environment variable, you must stop any tasks running on this container instance before stopping the agent\. Newer agents with the `ECS_DATADIR` environment variable save their state and you can stop and start them while tasks are running without issues\. For more information, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

1. Stop the `ecs` service:

   ```
   sudo stop ecs
   ```

   Output:

   ```
   ecs stop/waiting
   ```

1. Restart the `ecs` service\.
   + For the Amazon ECS\-optimized Amazon Linux 2 AMI:

     ```
     sudo systemctl restart ecs
     ```
   + For the Amazon ECS\-optimized Amazon Linux AMI:

     ```
     sudo stop ecs && sudo start ecs
     ```

1. \(Optional\) You can verify that the agent is running and see some information about your new container instance by querying the agent introspection API operation\. For more information, see [Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)\.

   ```
   curl http://localhost:51678/v1/metadata
   ```