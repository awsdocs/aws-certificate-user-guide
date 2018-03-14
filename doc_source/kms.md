# ACM Private Key Security<a name="kms"></a>

When you [request a certificate](gs-acm-request.md), AWS Certificate Manager \(ACM\) generates a public/private key pair\. For [imported certificates](import-certificate.md), you generate the key pair\. The public key becomes part of the certificate\. ACM stores the certificate and its corresponding private key, and uses AWS Key Management Service \(AWS KMS\) to help protect the private key\. The process works like this:

1. The first time you request or import a certificate in an AWS region, ACM creates an AWS\-managed customer master key \(CMK\) in AWS KMS with the alias **aws/acm**\. This CMK is unique in each AWS account and each AWS region\.

1. ACM uses this CMK to encrypt the certificate's private key\. ACM stores only an encrypted version of the private key \(ACM does not store the private key in plaintext form\)\. ACM uses the same CMK to encrypt the private keys for all certificates in a specific AWS account and a specific AWS region\.

1. When you associate the certificate with a service that is integrated with AWS Certificate Manager, ACM sends the certificate and the encrypted private key to the service\. You also implicitly create a grant in AWS KMS that allows the service to use the CMK in AWS KMS to decrypt the certificate's private key\. For more information about grants, see [ Using Grants ](http://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide*\. For more information about services supported by ACM, see [Services Integrated with AWS Certificate Manager](acm-services.md)\.

1. Integrated services use the CMK in AWS KMS to decrypt the private key\. Then the service uses the certificate and the decrypted \(plaintext\) private key to establish secure communication channels \(SSL/TLS sessions\) with its clients\.

1. When the certificate is disassociated from an integrated service, the grant created in step 3 is retired\. This means the service can no longer use the CMK in AWS KMS to decrypt the certificate's private key\.