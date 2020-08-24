# Inline Policies<a name="authen-inlinepolicies"></a>

Inline policies are policies that you create and manage and embed directly into a single user, group, or role\. The following policy examples show how to assign permissions to perform ACM actions\. For more information about attaching inline policies, see [Working with Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html) in the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\. You can use the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), or the IAM API to create and embed inline policies\. 

**Topics**
+ [Listing Certificates](#policy-list-certificates)
+ [Retrieving a Certificate](#policy-retrieve-certificates)
+ [Importing a Certificate](#policy-import-certificate)
+ [Deleting a Certificate](#policy-delete-certificates)
+ [Read\-Only Access to ACM](#policy-acm-read-only)
+ [Full Access to ACM](#policy-acm-full-access)
+ [Administrator Access to All AWS Resources](#policy-aws-administrator)

## Listing Certificates<a name="policy-list-certificates"></a>

The following policy allows a user to list all of the ACM certificates in the user's account\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"acm:ListCertificates",
         "Resource":"*"
      }
   ]
}
```

**Note**  
This permission is required for ACM certificates to appear in the Elastic Load Balancing and CloudFront consoles\. 

## Retrieving a Certificate<a name="policy-retrieve-certificates"></a>

The following policy allows a user to retrieve a specific ACM certificate\. 

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":"acm:GetCertificate",
      "Resource":"arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
   }
}
```

## Importing a Certificate<a name="policy-import-certificate"></a>

The following policy allows a user to import a certificate\. 

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":"acm:ImportCertificate",
      "Resource":"arn:aws:acm:ap-northeast-1:123456789012:certificate/01234567-89ab-cdef-0123-456789abcdef"
   }
}
```

## Deleting a Certificate<a name="policy-delete-certificates"></a>

The following policy allows a user to delete a specific ACM certificate\. 

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":"acm:DeleteCertificate",
      "Resource":"arn:aws:acm:us-east-1:123456789012:certificate/fedcba98-7654-3210-fedc-ba9876543210"
   }
}
```

## Read\-Only Access to ACM<a name="policy-acm-read-only"></a>

The following policy allows a user to describe and list an ACM certificate and to retrieve the ACM certificate and certificate chain\. 

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":[
         "acm:DescribeCertificate",
         "acm:ListCertificates",
         "acm:GetCertificate",
         "acm:ListTagsForCertificate"
      ],
      "Resource":"*"
   }
}
```

**Note**  
This policy is available as an AWS managed policy in the AWS Management Console\. For more information, see [AWSCertificateManagerReadOnly](authen-awsmanagedpolicies.md#acm-read-only-managed-policy)\. To view the managed policy in the console, go to [https://console\.aws\.amazon\.com/iam/home\#policies/arn:aws:iam::aws:policy/AWSCertificateManagerReadOnly](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AWSCertificateManagerReadOnly)\. 

## Full Access to ACM<a name="policy-acm-full-access"></a>

The following policy allows a user to perform any ACM action\. 

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
      }
   ]
}
```

**Note**  
This policy is available as an AWS managed policy in the AWS Management Console\. For more information, see [AWSCertificateManagerFullAccess](authen-awsmanagedpolicies.md#acm-full-access-managed-policy)\. To view the managed policy in the console, go to [https://console\.aws\.amazon\.com/iam/home\#policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess)\. 

## Administrator Access to All AWS Resources<a name="policy-aws-administrator"></a>

The following policy allows a user to perform any action on any AWS resource\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"*",
         "Resource":"*"
      }
   ]
}
```

**Note**  
This policy is available as an AWS managed policy in the AWS Management Console\. To view the managed policy in the console, go to [https://console\.aws\.amazon\.com/iam/home\#policies/arn:aws:iam::aws:policy/AdministratorAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AdministratorAccess)\. 