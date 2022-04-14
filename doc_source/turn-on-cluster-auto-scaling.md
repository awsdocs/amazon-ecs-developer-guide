# Turn on cluster Auto Scaling<a name="turn-on-cluster-auto-scaling"></a>

You can use the AWS CLI to turn on cluster Auto Scaling\. 

Before you begin, create an Auto Scaling group that uses managed scaling\. For more information, see [Creating an Auto Scaling group](asg-capacity-providers-create-auto-scaling-group.md)\.

After you create the Auto Scaling group, you need to associate it with the cluster and turn on managed scaling for the capacity provider\.

## Associate the capacity provider with the cluster<a name="associate-capacity-provider"></a>

Use the following steps to associate the capacity provider with the cluster\.

1. Run `put-cluster-capacity-providers` to associate one or more capacity providers with the cluster\. For more information, see `[put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs put-cluster-capacity-providers \
     --cluster ClusterName \
     --capacity-providers CapacityProviderName \
     --default-capacity-provider-strategy capacityProvider=CapacityProvider,weight=1
   ```

1. Run `describe-clusters` to verify that the association was successful\. For more information, see `[describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/ecs/describe-clusters.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs describe-clusters \
     --cluster ClusterName \
     --include ATTACHMENTS
   ```

## Turn on managed scaling for the capacity provider<a name="turn-on-managed-scaling"></a>

Use the following steps to turn on managed scaling for the capacity provider\.
+ Run `update-capacity-provider` to turn on managed auto scaling for the capacity provider\. For more information, see `[update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html)` in the *AWS CLI Command Reference*\.

  ```
  aws ecs update-capacity-provider \
    --cluster ClusterName \
    --capacity-providers CapacityProviderName \
    --auto-scaling-group-provider managedScaling=ENABLED
  ```