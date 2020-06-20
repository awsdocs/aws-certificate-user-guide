# Set Up AWS and IAM<a name="setup-aws-iam"></a>

Before you can use ACM, you must sign up for Amazon Web Services\. As a best practice, you can create an IAM user to limit the actions your users can perform\. 

## Sign Up for AWS<a name="setup-aws"></a>

If you are not already an Amazon Web Services \(AWS\) customer, you must sign up to be able to use ACM\. Your account is automatically signed up for all available services, but you are charged for only the services that you use\. Also, if you are a new AWS customer, you can get started for free\. For more information, see [AWS Free Tier](https://aws.amazon.com/free/)\. 

**To sign up for an AWS account**

1. Go to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choose **Sign Up**\. 

1. Follow the on\-screen instructions\.

**Note**  
Part of the sign\-up procedure includes receiving an automated telephone call and entering the supplied PIN on the telephone keypad\. You must also supply a credit card number even if you are signing up for the free tier\. 

## Create an IAM User<a name="setup-iam"></a>

All AWS accounts have root user credentials \(that is, the credentials of the account owner\)\. These credentials allow full access to all resources in the account\. Because you can't restrict permissions for root user credentials, we recommend that you delete your root user access keys\. Then create AWS Identity and Access Management \(IAM\) user credentials for everyday interaction with AWS\. For more information, see [Lock away your AWS account \(root\) access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials) in the *IAM User Guide*\.

**Note**  
You may need AWS account root user access for specific tasks, such as changing an AWS support plan or closing your account\. In these cases, sign in to the AWS Management Console with your email and password\. See [Email and Password \(Root User\)](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#email-and-password-for-your-AWS-account)\.

For a list of tasks that require root user access, see [AWS Tasks That Require AWS Account Root User](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

With IAM, you can securely control access to AWS services and resources for users in your AWS account\. For example, if you require administrator\-level permissions, you can create an IAM user, grant that user full access, and then use those credentials to interact with AWS\. If you need to modify or revoke your permissions, you can delete or modify the policies that are associated with that IAM user\.

If you have multiple users that require access to your AWS account, you can create unique credentials for each user and define who has access to which resources\. You don't need to share credentials\. For example, you can create IAM users with read\-only access to resources in your AWS account and distribute those credentials to your users\. 

ACM also provides two [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) that you can use:
+ **AWSCertificateManagerFullAccess**
+ **AWSCertificateManagerReadOnly**

**Note**  
Any activity or costs that are associated with the IAM user are billed to the AWS account owner\.