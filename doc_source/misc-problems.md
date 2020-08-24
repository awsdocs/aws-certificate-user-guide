# Troubleshooting Other Problems<a name="misc-problems"></a>

This section includes guidance for problems not related to issuing or validating ACM certificates\.

**Topics**
+ [Troubleshoot Certificate Import Problems](troubleshoot-import.md)
+ [Troubleshoot Certificate Pinning Problems](troubleshooting-pinning.md)
+ [Troubleshoot API Gateway Problems](troubleshoot-apigateway.md)
+ [What to Do when a Working Certificate Fails Unexpectedly](#unexpected-failure)
+ [Problems with the ACM Service\-Linked Role \(SLR\)](slr-problems.md)

## What to Do when a Working Certificate Fails Unexpectedly<a name="unexpected-failure"></a>

If you have successfully associated an ACM certificate with an integrated service, but the certificate stops working and the integrated service begins returning errors, the cause may be a change in the permissions that the service needs in order to use an ACM certificate\. 

For example, Elastic Load Balancing \(ELB\) requires permission to decrypt an AWS KMS Customer Managed Key \(CMK\) that, in turn, decrypts the certificate's private key\. This permission is granted by a resource\-based policy that ACM applies when you associate a certificate with ELB\. If ELB loses the grant for that permission, it will fail the next time it attempts to decrypt the certificate key\.

To investigate the problem, check the status of your grants using the AWS KMS console at [https://console.aws.amazon.com/kms](https://console.aws.amazon.com/kms)\. Then take one of the following actions:
+ If you believe that permissions granted to an integrated service have been revoked, visit the integrated service's console, disassociate the certificate from the service, then re\-associate it\. This will reapply the resource\-based policy and put a new grant in place\.
+ If you believe that permissions granted to ACM have been revoked, contact AWS Support at https://console\.aws\.amazon\.com/support/home\#/\.