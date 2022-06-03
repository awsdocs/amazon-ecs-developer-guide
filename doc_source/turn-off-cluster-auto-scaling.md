# Turn off cluster auto scaling<a name="turn-off-cluster-auto-scaling"></a>

You can use the AWS CLI to turn off cluster auto scaling\. 

To turn off cluster auto scaling for a cluster, you can either disassociate the capacity provider with managed scaling turned on from the cluster or by updating the capacity provider to turn off managed scaling\.

## Disassociate the capacity provider with the cluster<a name="disassociate-capacity-provider"></a>

Use the following steps to disassociate a capacity provider with a cluster\.

1. Use the `put-cluster-capacity-providers` command to disassociate the Auto Scaling group capacity provider with the cluster\. The cluster can keep the association with the AWS Fargate capacity providers\. For more information, see `[put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs put-cluster-capacity-providers \
     --cluster ClusterName \
     --capacity-providers FARGATE FARGATE_SPOT \
     --default-capacity-provider-strategy '[]'
   ```

1. Use the `describe-clusters` command to verify that the disassociation was successful\. For more information, see `[describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/ecs/describe-clusters.html)` in the *AWS CLI Command Reference*\.

   ```
   aws ecs describe-clusters \
     --cluster ClusterName \
     --include ATTACHMENTS
   ```

## Turn off managed scaling for the capacity provider<a name="turn-off-managed-scaling"></a>

Use the following steps to turn off managed scaling for the capacity provider\.
+ Use the `update-capacity-provider` command to turn off managed auto scaling for the capacity provider\. For more information, see `[update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html)` in the *AWS CLI Command Reference*\.

  ```
  aws ecs update-capacity-provider \
    --cluster ClusterName \
    --capacity-providers CapacityProviderName \
    --auto-scaling-group-provider managedScaling=DISABLED
  ```