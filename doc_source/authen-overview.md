# Overview of Managing Access to Your ACM Resources<a name="authen-overview"></a>

Every AWS Certificate Manager \(ACM\) resource belongs to an AWS account, and permissions to create or access the resources are defined in permissions policies in that account\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. Some services \(including ACM\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator permissions\. For more information, see [Creating an Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\. 

When managing permissions, you decide who gets the permissions, the resources they get permissions for, and the specific actions allowed\.

**Topics**
+ [ACM Resources and Operations](#acm-resources-operations)
+ [Understanding Resource Ownership](#understand-resource-ownership)
+ [Managing Access to ACM Certificates](#managing-access)

## ACM Resources and Operations<a name="acm-resources-operations"></a>

In ACM, the primary resource is a *certificate*\. Certificates have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following list\. 
+ **ACM Certificate**

  ARN format:

   ` arn:aws:acm:AWS region:AWS account ID:certificate/Certificate ID ` 

  Example ARN:

   ` arn:aws:acm:us-west-2:123456789012:certificate/12345678-12ab-34cd-56ef-12345678 ` 

## Understanding Resource Ownership<a name="understand-resource-ownership"></a>

A *resource owner* is the AWS account that created a resource\. That is, the resource owner is the AWS account of the *principal entity* that authenticates the request that created the resource\. \(A principle entity can be an AWS account root user, an IAM user, or an IAM role\.\) The following examples illustrate how this works\. 
+  If you use the credentials of your AWS account root user to create an ACM certificate, your AWS account owns the certificate\. 
+  If you create an IAM user in your AWS account, you can grant that user permission to create an ACM certificate\. However, the account to which that user belongs owns the certificate\. 
+  If you create an IAM role in your AWS account and grant it permission to create an ACM certificate, anyone who can assume the role can create a certificate\. However, the account to which the role belongs owns the certificate\. 

## Managing Access to ACM Certificates<a name="managing-access"></a>

A *permissions policy* describes who has access to what\. This section explains the available options for creating permissions policies\. 

**Note**  
This section discusses using IAM in the context of ACM\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html)\. 

 You can use IAM to create policies that apply permissions to IAM users, groups, and roles\. These are called *identity–based policies*\. IAM offers the following types of identity–based policies: 
+ **AWS managed policies** – Policies that are created and managed by AWS\. These are standalone policies that you can attach to multiple users, groups, and roles in your AWS account\. 
+  **Customer managed policies** – Policies that you create and manage in your AWS account and which you can attach to multiple users, groups, and roles\. You have more precise control when using customer managed policies than you have when using AWS managed policies\. 
+  **Inline policies** – Policies that you create and manage and which you embed directly into a single user, group, or role\. 

Other services, such as Amazon S3, also support resource–based permissions policies\. For example, you can attach a policy to an Amazon S3 bucket to manage access permissions to that bucket\. ACM does not support resource\-based policies\. 