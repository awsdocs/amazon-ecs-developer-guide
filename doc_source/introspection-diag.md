# Agent introspection diagnostics<a name="introspection-diag"></a>

The Amazon ECS agent introspection API can provide helpful diagnostic information\. For example, you can use the agent introspection API to get the Docker ID for a container in your task\. You can use the agent introspection API by connecting to a container instance using SSH\. For more information, see [Connect to your container instance](instance-connect.md)\.

**Important**  
Your container instance must have an IAM role that allows access to Amazon ECS in order to reach the introspection API\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

The below example shows two tasks, one that is currently running and one that was stopped\.

**Note**  
The command below is piped through the python \-mjson\.tool for greater readability\.

```
curl http://localhost:51678/v1/tasks | python -mjson.tool
```

Output:

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1095  100  1095    0     0   117k      0 --:--:-- --:--:-- --:--:--  133k
{
    "Tasks": [
        {
            "Arn": "arn:aws:ecs:us-west-2:aws_account_id:task/090eff9b-1ce3-4db6-848a-a8d14064fd24",
            "Containers": [
                {
                    "DockerId": "189a8ff4b5f04affe40e5160a5ffadca395136eb5faf4950c57963c06f82c76d",
                    "DockerName": "ecs-console-sample-app-static-6-simple-app-86caf9bcabe3e9c61600",
                    "Name": "simple-app"
                },
                {
                    "DockerId": "f7f1f8a7a245c5da83aa92729bd28c6bcb004d1f6a35409e4207e1d34030e966",
                    "DockerName": "ecs-console-sample-app-static-6-busybox-ce83ce978a87a890ab01",
                    "Name": "busybox"
                }
            ],
            "Family": "console-sample-app-static",
            "KnownStatus": "STOPPED",
            "Version": "6"
        },
        {
            "Arn": "arn:aws:ecs:us-west-2:aws_account_id:task/1810e302-eaea-4da9-a638-097bea534740",
            "Containers": [
                {
                    "DockerId": "dc7240fe892ab233dbbcee5044d95e1456c120dba9a6b56ec513da45c38e3aeb",
                    "DockerName": "ecs-console-sample-app-static-6-simple-app-f0e5859699a7aecfb101",
                    "Name": "simple-app"
                },
                {
                    "DockerId": "096d685fb85a1ff3e021c8254672ab8497e3c13986b9cf005cbae9460b7b901e",
                    "DockerName": "ecs-console-sample-app-static-6-busybox-92e4b8d0ecd0cce69a01",
                    "Name": "busybox"
                }
            ],
            "DesiredStatus": "RUNNING",
            "Family": "console-sample-app-static",
            "KnownStatus": "RUNNING",
            "Version": "6"
        }
    ]
}
```

In the above example, the stopped task \(*090eff9b\-1ce3\-4db6\-848a\-a8d14064fd24*\) has two containers\. You can use docker inspect *container\-ID* to view detailed information on each container\. For more information, see [Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)\.