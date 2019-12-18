# Amazon ECR Managed Policies<a name="ecr_managed_policies"></a>

Amazon ECR provides several managed policies that you can attach to IAM users or EC2 instances that allow differing levels of control over Amazon ECR resources and API operations\. You can apply these policies directly, or you can use them as starting points for creating your own policies\. For more information about each API operation mentioned in these policies, see [Actions](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

**Topics**
+ [`AmazonEC2ContainerRegistryFullAccess`](#AmazonEC2ContainerRegistryFullAccess)
+ [`AmazonEC2ContainerRegistryPowerUser`](#AmazonEC2ContainerRegistryPowerUser)
+ [`AmazonEC2ContainerRegistryReadOnly`](#AmazonEC2ContainerRegistryReadOnly)

## `AmazonEC2ContainerRegistryFullAccess`<a name="AmazonEC2ContainerRegistryFullAccess"></a>

This managed policy is a starting point for customers who are looking to provide an IAM user or role with full administrator access to manage their use of Amazon ECR\. The [Amazon ECR Lifecycle Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html) feature enables customers to specify the lifecycle management of images in a repository\. Lifecycle policy events are reported as CloudTrail events, and Amazon ECR is integrated with AWS CloudTrail to display a customer's lifecycle policy events directly in the Amazon ECR console\. The `AmazonEC2ContainerRegistryFullAccess` managed IAM policy includes the `cloudtrail:LookupEvents` permission to facilitate this behavior\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:*",
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

## `AmazonEC2ContainerRegistryPowerUser`<a name="AmazonEC2ContainerRegistryPowerUser"></a>

This managed policy allows power user access to Amazon ECR, which allows read and write access to repositories, but does not allow users to delete repositories or change the policy documents applied to them\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:ListTagsForResource",
                "ecr:DescribeImageScanFindings",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:PutImage"
            ],
            "Resource": "*"
        }
    ]
}
```

## `AmazonEC2ContainerRegistryReadOnly`<a name="AmazonEC2ContainerRegistryReadOnly"></a>

This managed policy allows read\-only access to Amazon ECR, such as the ability to list repositories and the images within the repositories, and also to pull images from Amazon ECR with the Docker CLI\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:ListTagsForResource",
                "ecr:DescribeImageScanFindings"
            ],
            "Resource": "*"
        }
    ]
}
```