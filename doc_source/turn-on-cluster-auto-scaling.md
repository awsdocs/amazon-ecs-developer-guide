# Turn on cluster Auto Scaling<a name="turn-on-cluster-auto-scaling"></a>

You can use the AWS CLI to turn on cluster Auto Scaling\. 

Before you begin, create an Auto Scaling group and a capacity provider\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

## Associate the capacity provider with the cluster<a name="associate-capacity-provider"></a>

Use the following steps to associate the capacity provider with the cluster\.

1. Use the `put-cluster-capacity-providers` command to associate one or more capacity providers with the cluster\. To add the AWS Fargate capacity providers, simply include the `FARGATE` and `FARGATE_SPOT` capacity providers in the request\. For more information, see `[put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs put-cluster-capacity-providers \
     --cluster ClusterName \
     --capacity-providers CapacityProviderName FARGATE FARGATE_SPOT \
     --default-capacity-provider-strategy capacityProvider=CapacityProvider,weight=1
   ```

1. Use the `describe-clusters` command to verify that the association was successful\. For more information, see `[describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/ecs/describe-clusters.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs describe-clusters \
     --cluster ClusterName \
     --include ATTACHMENTS
   ```

## Turn on managed scaling for the capacity provider<a name="turn-on-managed-scaling"></a>

Use the following steps to turn on managed scaling for the capacity provider\.
+ Use the `update-capacity-provider` command to turn on managed auto scaling for the capacity provider\. For more information, see `[update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html)` in the *AWS CLI Command Reference*\.

  ```
  aws ecs update-capacity-provider \
    --cluster ClusterName \
    --capacity-providers CapacityProviderName \
    --auto-scaling-group-provider managedScaling=ENABLED
  ```