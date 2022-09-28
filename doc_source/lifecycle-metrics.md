# Use CloudWatch Container Insights to view Amazon ECS lifecycle events<a name="lifecycle-metrics"></a>

You can view Amazon ECS task and service lifecycle events within the CloudWatch Container Insights console\. This helps you correlate your container metrics, logs, and events in a single view to give you a more complete operational visibility\. 

The events that you can view are the ones that Amazon ECS sends to Amazon EventBridge\. For more information, see [Amazon ECS events](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_cwe_events.html)\.

You can choose to configure performance metrics for clusters, tasks, or services\. Depending on the resource you choose, the following events are reported:
+ Container instance state change events
+ Service action events
+ Task state change events

You must configure the correct permissions, and then you can configure and view the events in the CloudWatch Container Insights console\. For more information, see [Amazon ECS lifecycle events within Container Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/container-insights-ECS-lifecycle-events.html) in the *Amazon CloudWatch User Guide*\.

## Permissions required to configure Container Insights to view Amazon ECS lifecycle events<a name="required-permissions-configure"></a>

The following permissions are required to configure the lifecycle events:
+ events:PutRule
+ events:PutTargets
+ logs:CreateLogGroup

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "events:PutRule",
        "events:PutTargets",
        "logs:CreateLogGroup"
      ],
      "Resource": "*"
    }
  ]
}
```

## Permissions required to view Amazon ECS lifecycle events in Container Insights<a name="required-permissions-view"></a>

The following permissions are required to view the lifecycle events:
+ events:DescribeRule
+ events:ListTargetsByRule
+ logs:DescribeLogGroups

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "events:events:DescribeRule",
        "events:PutTargets",
        "logs:DescribeLogGroups"
      ],
      "Resource": "*"
    }
  ]
}
```