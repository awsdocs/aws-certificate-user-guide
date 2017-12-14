# Logging AWS Certificate Manager API Calls with AWS CloudTrail<a name="ct-acm"></a>

AWS Certificate Manager \(ACM\) is integrated with AWS CloudTrail, a service that captures API calls, delivers the log files to an Amazon Simple Storage Service \(Amazon S3\) bucket that you specify, and maintains API call history\. CloudTrail captures API calls from the AWS Certificate Manager console, CLI, or from your code\. Using the information collected by CloudTrail, you can determine the request that was made to ACM, the IP address from which the request was made, who made the request, when it was made, and so on\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)\. 

When you enable CloudTrail logging in your AWS account, API calls made to ACM actions are tracked in CloudTrail log files\. The ACM records are written with other AWS service records\. CloudTrail determines when to create and write to a new log file based on a time period and file size\. 

The following ACM actions are supported:

+ [AddTagsToCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html)

+ [DeleteCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html)

+ [DescribeCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html)

+ [GetCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html)

+ [ImportCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html)

+ [ListCertificates](http://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html)

+ [ListTagsForCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html)

+ [RemoveTagsFromCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html)

+ [RequestCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html)

+ [ResendValidationEmail](http://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html)

Every log entry contains information about who generated the request\. The user identity information in the log entry helps you determine whether the request was made with root or with IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\. 

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted using Amazon S3 server\-side encryption \(SSE\)\. 

You can choose to have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log delivery\. For more information, see [ Configuring Amazon SNS Notifications for CloudTrail ](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/configure-sns-notifications-for-cloudtrail.html) in the *AWS CloudTrail User Guide*\. 

You can also aggregate AWS Certificate Manager log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [ Receiving CloudTrail Log Files from Multiple Regions ](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\. 

CloudTrail log files contain one or more log entries where each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered trace of the public API calls\. For more information about the fields that make up a log entry, see the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html)\. 

For examples of possible ACM CloudTrail entries, see the following topics\.


+ [Adding Tags to a Certificate](ct-acm-addtags.md)
+ [Deleting a Certificate](ct-acm-delete.md)
+ [Describing a Certificate](ct-acm-describe.md)
+ [Retrieving a Certificate](ct-acm-get.md)
+ [Import a Certificate](ct-acm-import.md)
+ [Listing Certificates](ct-acm-list.md)
+ [Listing Tags for a Certificate](ct-acm-listtags.md)
+ [Removing Tags from a Certificate](ct-acm-removetag.md)
+ [Requesting a Certificate](ct-acm-request.md)
+ [Resending Validation Email](ct-acm-resendmail.md)