# Container instance health<a name="container-instance-health"></a>

Amazon ECS provides container instance health monitoring\. You can quickly determine whether Amazon ECS has detected any problems that might prevent your container instances from running containers\. Amazon ECS performs automated checks on every running container instance with agent version `1.57.0` or later to identify issues\. For more information on verifying the agent version an a container instance, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.

Status checks are performed about twice per minute, returning a pass or a fail status\. If all checks pass, the overall status of the instance is `OK`\. If one or more checks fail, the overall status is `IMPAIRED`\. Status checks are built into Amazon ECS container agent, so they cannot be disabled or deleted\. You can view the results of these status checks to identify specific and detectable problems\.

The container instance health status can be retrieved using the `DescribeContainerInstances` API\. The following AWS CLI command retrieves the container instance health status\.

```
aws ecs describe-container-instances \
     --cluster cluster_name \
     --container-instances 47279cd2cadb41cbaef2dcEXAMPLE \
     --include CONTAINER_INSTANCE_HEALTH
```

The following is an example of the health status object in the output\.

```
"healthStatus": {
	"overallStatus": "OK",
	"details": [{
		"type": "CONTAINER_RUNTIME",
		"status": "OK",
		"lastUpdated": "2021-11-10T03:30:26+00:00",
		"lastStatusChange": "2021-11-10T03:26:41+00:00"
	}]
}
```