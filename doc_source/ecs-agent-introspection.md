# Amazon ECS Container Agent Introspection<a name="ecs-agent-introspection"></a>

The Amazon ECS container agent provides an API operation for gathering details about the container instance on which the agent is running and the associated tasks running on that instance\. You can use the curl command from within the container instance to query the Amazon ECS container agent \(port 51678\) and return container instance metadata or task information\.

**Important**  
Your container instance must have an IAM role that allows access to Amazon ECS in order to retrieve the metadata\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

To view container instance metadata, log in to your container instance via SSH and run the following command\. Metadata includes the container instance ID, the Amazon ECS cluster in which the container instance is registered, and the Amazon ECS container agent version information\.

```
curl -s http://localhost:51678/v1/metadata | python -mjson.tool
```

Output:

```
{
    "Cluster": "cluster_name",
    "ContainerInstanceArn": "arn:aws:ecs:region:aws_account_id:container-instance/cluster_name/container_instance_id",
    "Version": "Amazon ECS Agent - v1.30.0 (02ff320c)"
}
```

To view information about all of the tasks that are running on a container instance, log in to your container instance via SSH and run the following command:

```
curl http://localhost:51678/v1/tasks
```

Output:

```
{
  "Tasks": [
    {
      "Arn": "arn:aws:ecs:us-west-2:012345678910:task/default/example5-58ff-46c9-ae05-543f8example",
      "DesiredStatus": "RUNNING",
      "KnownStatus": "RUNNING",
      "Family": "hello_world",
      "Version": "8",
      "Containers": [
        {
          "DockerId": "9581a69a761a557fbfce1d0f6745e4af5b9dbfb86b6b2c5c4df156f1a5932ff1",
          "DockerName": "ecs-hello_world-8-mysql-fcae8ac8f9f1d89d8301",
          "Name": "mysql"
        },
        {
          "DockerId": "bf25c5c5b2d4dba68846c7236e75b6915e1e778d31611e3c6a06831e39814a15",
          "DockerName": "ecs-hello_world-8-wordpress-e8bfddf9b488dff36c00",
          "Name": "wordpress"
        }
      ]
    }
  ]
}
```

You can view information for a particular task that is running on a container instance\. To specify a specific task or container, append one of the following to the request:
+ The task ARN \(`?taskarn=task_arn`\)
+ The Docker ID for a container \(`?dockerid=docker_id`\)

 To get task information with a container's Docker ID, log in to your container instance via SSH and run the following command\.

**Note**  
Amazon ECS container agents before version 1\.14\.2 require full Docker container IDs for the introspection API, not the short version that is shown with docker ps\. You can get the full Docker ID for a container by running the docker ps \-\-no\-trunc command on the container instance\.

```
curl http://localhost:51678/v1/tasks?dockerid=79c796ed2a7f
```

Output:

```
{
    "Arn": "arn:aws:ecs:us-west-2:012345678910:task/default/e01d58a8-151b-40e8-bc01-22647b9ecfec",
    "Containers": [
        {
            "DockerId": "79c796ed2a7f864f485c76f83f3165488097279d296a7c05bd5201a1c69b2920",
            "DockerName": "ecs-nginx-efs-2-nginx-9ac0808dd0afa495f001",
            "Name": "nginx"
        }
    ],
    "DesiredStatus": "RUNNING",
    "Family": "nginx-efs",
    "KnownStatus": "RUNNING",
    "Version": "2"
}
```