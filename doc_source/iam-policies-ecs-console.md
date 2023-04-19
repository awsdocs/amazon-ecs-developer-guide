# Example policies<a name="iam-policies-ecs-console"></a>

You can use IAM policies to grant users permissions to view and work with specific resources in the Amazon ECS console\. You can use the example policies in the previous section; however, they are designed for requests that are made with the AWS CLI or an AWS SDK\.

## Example: Allow users to delete a cluster based on tags<a name="ex-delete-tags"></a>

The following policy allows users to delete clusters when the tag has a key/value pair of "Purpose/Testing"\.

```
{
  "Version": "2012-10-17",
  "Statement": [
       {
      "Action": [
        "ec2:DeleteCluster"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:ecs:region:account-id:cluster/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Purpose": "Testing"
        }
      }
    }
  ]
}
```