# Amazon ECR Managed Policies<a name="ecr_managed_policies"></a>

Amazon ECR provides several managed policies that you can attach to IAM users or EC2 instances that allow differing levels of control over Amazon ECR resources and API operations\. You can apply these policies directly, or you can use them as starting points for creating your own policies\. For more information about each API operation mentioned in these policies, see [Actions](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

**Topics**
+ [AmazonEC2ContainerRegistryFullAccess](#AmazonEC2ContainerRegistryFullAccess)
+ [AmazonEC2ContainerRegistryPowerUser](#AmazonEC2ContainerRegistryPowerUser)
+ [AmazonEC2ContainerRegistryReadOnly](#AmazonEC2ContainerRegistryReadOnly)

## AmazonEC2ContainerRegistryFullAccess<a name="AmazonEC2ContainerRegistryFullAccess"></a>

This managed policy allows full administrator access to Amazon ECR\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:*"
      ],
      "Resource": "*"
    }
  ]
}
```

## AmazonEC2ContainerRegistryPowerUser<a name="AmazonEC2ContainerRegistryPowerUser"></a>

This managed policy allows power user access to Amazon ECR, which allows read and write access to repositories, but does not allow users to delete repositories or change the policy documents applied to them\. 

```
{
	"Version": "2012-10-17",
	"Statement": [{
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
			"ecr:InitiateLayerUpload",
			"ecr:UploadLayerPart",
			"ecr:CompleteLayerUpload",
			"ecr:PutImage"
		],
		"Resource": "*"
	}]
}
```

## AmazonEC2ContainerRegistryReadOnly<a name="AmazonEC2ContainerRegistryReadOnly"></a>

This managed policy allows read\-only access to Amazon ECR, such as the ability to list repositories and the images within the repositories, and also to pull images from Amazon ECR with the Docker CLI\. 

```
{
	"Version": "2012-10-17",
	"Statement": [{
		"Effect": "Allow",
		"Action": [
			"ecr:GetAuthorizationToken",
			"ecr:BatchCheckLayerAvailability",
			"ecr:GetDownloadUrlForLayer",
			"ecr:GetRepositoryPolicy",
			"ecr:DescribeRepositories",
			"ecr:ListImages",
			"ecr:DescribeImages",
			"ecr:BatchGetImage"
		],
		"Resource": "*"
	}]
}
```