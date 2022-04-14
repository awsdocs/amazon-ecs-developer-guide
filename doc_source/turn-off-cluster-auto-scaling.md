# Turn off cluster Auto Scaling<a name="turn-off-cluster-auto-scaling"></a>

You can use the AWS CLI to turn off cluster Auto Scaling\. 

You can turn off cluster Auto Scaling by using either of the following options:
+ Disassociate the capacity provider from the cluster
+ Turn off managed scaling for the capacity provider

## Disassociate the capacity provider with the cluster<a name="disassociate-capacity-provider"></a>

Use the following steps to associate the capacity provider with the cluster\.

1. Run `put-cluster-capacity-providers` to associate one or more capacity providers with the cluster\. For more information, see `[put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs put-cluster-capacity-providers \
     --cluster ClusterName \
     --capacity-providers [] \
     --default-capacity-provider-strategy []
   ```

1. Run `describe-clusters` to verify that the association was successful\. For more information, see `[describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/ecs/describe-clusters.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs describe-clusters \
     --cluster ClusterName \
     --include ATTACHMENTS
   ```

## Turn off managed scaling for the capacity provider<a name="turn-off-managed-scaling"></a>

Use the following steps to turn off managed scaling for the capacity provider\.
+ Run `update-capacity-provider` to turn off managed auto scaling for the capacity provider\. For more information, see `[update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html)` in the *AWS CLI Command Reference*\.

  ```
  aws ecs update-capacity-provider \
    --cluster ClusterName \
    --capacity-providers CapacityProviderName \
    --auto-scaling-group-provider managedScaling=DISABLED
  ```