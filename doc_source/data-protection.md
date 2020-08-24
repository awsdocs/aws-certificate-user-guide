# Data Protection in AWS Certificate Manager<a name="data-protection"></a>

AWS Certificate Manager \(ACM\) conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN Partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with ACM or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into ACM or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

## ACM Private Key Security<a name="kms"></a>

When you [request a public certificate](gs-acm-request-public.md), AWS Certificate Manager \(ACM\) generates a public/private key pair\. For [imported certificates](import-certificate.md), you generate the key pair\. The public key becomes part of the certificate\. ACM stores the certificate and its corresponding private key, and uses AWS Key Management Service \(AWS KMS\) to help protect the private key\. The process works like this:

1. The first time you request or import a certificate in an AWS Region, ACM creates an AWS managed customer master key \(CMK\) in AWS KMS with the alias **aws/acm**\. This CMK is unique in each AWS account and each AWS Region\.

1. ACM uses this CMK to encrypt the certificate's private key\. ACM stores only an encrypted version of the private key; ACM does not store the private key in plaintext form\. ACM uses the same CMK to encrypt the private keys for all certificates in a specific AWS account and a specific AWS Region\.

1. When you associate the certificate with a service that is integrated with AWS Certificate Manager, ACM sends the certificate and the encrypted private key to the service\. A grant is also created in AWS KMS that allows the service to use the CMK in AWS KMS to decrypt the certificate's private key\. For more information about grants, see [ Using Grants ](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide*\. For more information about services supported by ACM, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 
**Note**  
You have control over the automatically created AWS KMS grant\. If you delete this grant for any reason, you lose ACM functionality for the integrated service\.

1. Integrated services use the CMK in AWS KMS to decrypt the private key\. Then the service uses the certificate and the decrypted \(plaintext\) private key to establish secure communication channels \(SSL/TLS sessions\) with its clients\.

1. When the certificate is disassociated from an integrated service, the grant created in step 3 is retired\. This means the service can no longer use the CMK in AWS KMS to decrypt the certificate's private key\.