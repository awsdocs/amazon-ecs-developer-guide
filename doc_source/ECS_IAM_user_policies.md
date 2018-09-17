# Creating Amazon ECS IAM Policies<a name="ECS_IAM_user_policies"></a>

You can create specific IAM policies to restrict the calls and resources that users in your account have access to, and then attach those policies to IAM users\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\. For more general information about IAM policies, see [Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. For more information about managing and creating custom IAM policies, see [Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html)\.

**To create an IAM policy for a user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, **Create policy**\. 

1. On the **Visual editor** tab, choose **Choose a Service** and select **Elastic Container Service**\.

1. Choose **Select actions** and then choose the actions to add to the policy\. For example policies, see [Amazon ECS IAM Policy Examples](IAMPolicyExamples.md)\.

1. \(Optional\) Choose **Specify request conditions \(optional\)** to add conditions to the policy that you are creating\. Conditions limit a JSON policy statement's effect\. For example, you can specify that a user is allowed to perform the actions on the resources only when that user's request happens within a certain time range\. You can also use commonly used conditions to limit whether a user must be authenticated using a multi\-factor authentication \(MFA\) device, or if the request must originate from within a certain range of IP addresses\. For lists of all of the context keys that you can use in a policy condition, see [AWS Service Actions and Condition Context Keys for Use in IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html)\.

1. Choose **Review policy**\.

1. In the **Name** field, type your own unique name, such as `AmazonECSUserPolicy`\.

1. Choose **Create Policy** to finish\. 

**To attach an IAM policy to a user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users** and then choose the user you would like to attach the policy to\. 

1. Choose **Permissions**, **Add permissions**\.

1. In the **Grant permissions** section, choose **Attach existing policies directly**\.

1. Select the custom policy that you created in the previous procedure and choose **Next: Review**\.

1. Review your details and choose **Add permissions** to finish\.