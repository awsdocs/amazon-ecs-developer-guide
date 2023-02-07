# Modifying account settings using the classic console<a name="ecs-modifying-longer-id-settings-classic"></a>


|  | 
| --- |
| The new experience is now the default in the Amazon ECS console\. For more information, see [Viewing account settings using the console](ecs-viewing-longer-id-settings.md)\. | 

You can manage your account settings using the classic console\. 

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to modify your account settings\.

1. From the dashboard, choose **Account Settings**\.

1. On the **Amazon ECS ARN and resource ID settings**, **AWSVPC Trunking**, and **CloudWatch Container Insights** sections, you can select or deselect the check boxes for each account setting for the authenticated IAM user and role\. Choose **Save** once finished\.
**Important**  
IAM users and IAM roles need the `ecs:PutAccountSetting` permission to perform this action\.

1. On the confirmation screen, choose **Confirm** to save the selection\.