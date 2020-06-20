# Using CloudTrail with AWS Certificate Manager<a name="cloudtrail"></a>

AWS Certificate Manager is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in ACM\. CloudTrail is enabled by default on your AWS account\. CloudTrail captures API calls for ACM as events, including calls from the ACM console and code calls to the ACM API operations\. If you configure a *trail*, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for ACM\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. 

Using the information collected by CloudTrail, you can determine the request that was made to ACM, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. When supported event activity occurs in ACM, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. 

Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. 

For more information about CloudTrail, consult the following documentation: 
+ [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

**Topics**
+ [ACM API Actions Supported in CloudTrail Logging](acm-supported-actions-in-cloudtrail.md)
+ [Logging for ACM\-Related API Calls](ct-related.md)