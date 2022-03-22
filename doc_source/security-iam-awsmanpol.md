# AWS managed policies for AWS Certificate Manager<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

**Topics**
+ [AWSCertificateManagerReadOnly](#acm-read-only-managed-policy)
+ [AWSCertificateManagerFullAccess](#acm-full-access-managed-policy)
+ [ACM updates to AWS managed policies](#security-iam-awsmanpol-updates)

## AWSCertificateManagerReadOnly<a name="acm-read-only-managed-policy"></a>

This policy provides read–only access to ACM certificates; it allows users to describe, list, and retrieve ACM certificates\. 

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":[
         "acm:DescribeCertificate",
         "acm:ListCertificates",
         "acm:GetCertificate",
         "acm:ListTagsForCertificate",
         "acm:GetAccountConfiguration"
      ],
      "Resource":"*"
   }
}
```

To view this AWS managed policy in the console, go to [https://console\.aws\.amazon\.com/iam/home\#policies/arn:aws:iam::aws:policy/AWSCertificateManagerReadOnly](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AWSCertificateManagerReadOnly)\. 

## AWSCertificateManagerFullAccess<a name="acm-full-access-managed-policy"></a>

 This policy provides full access to all ACM actions and resources\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "acm:*"
         ],
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":"iam:CreateServiceLinkedRole",
         "Resource":"arn:aws:iam::*:role/aws-service-role/acm.amazonaws.com/AWSServiceRoleForCertificateManager*",
         "Condition":{
            "StringEquals":{
               "iam:AWSServiceName":"acm.amazonaws.com"
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "iam:DeleteServiceLinkedRole",
            "iam:GetServiceLinkedRoleDeletionStatus",
            "iam:GetRole"
         ],
         "Resource":"arn:aws:iam::*:role/aws-service-role/acm.amazonaws.com/AWSServiceRoleForCertificateManager*"
      }
   ]
}
```

To view this AWS managed policy in the console, go to [https://console\.aws\.amazon\.com/iam/home\#policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess)\. 

## ACM updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for ACM since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the ACM [Document history](dochistory.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
| Added GetAccountConfiguration support to the [AWSCertificateManagerReadOnly](https://docs.aws.amazon.com/acm/latest/userguide/acm-read-only-managed-policy.html) policy\. | The AWSCertificateManagerReadOnly policy now includes permission to call the GetAccountConfiguration API action\. | March 3, 2021 | 
|  ACM starts tracking changes  |  ACM starts tracking changes for AWS managed policies\.  | March 3, 2021 | 
