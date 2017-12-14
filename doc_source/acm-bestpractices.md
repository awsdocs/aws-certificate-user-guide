# Best Practices<a name="acm-bestpractices"></a>

 Best practices are recommendations that can help you use AWS Certificate Manager \(AWS Certificate Manager\) more effectively\. The following best practices are based on real\-world experience from current ACM customers\. 


+ [AWS CloudFormation](#best-practices-cloudformation)
+ [Certificate Pinning](#best-practices-pinning)
+ [Domain Validation](#best-practices-validating)
+ [Adding or Deleting Domain Names](#best-practices-add-delete)
+ [Requesting a Limit Increase](#best-practices-limit-increase)

## AWS CloudFormation<a name="best-practices-cloudformation"></a>

 With AWS CloudFormation you can create a template that describes the AWS resources that you want to use\. AWS CloudFormation then provisions and configures those resources for you\. AWS CloudFormation can provision resources that are supported by ACM such as Elastic Load Balancing, Amazon CloudFront, and Amazon API Gateway\. For more information, see [Services Integrated with AWS Certificate Manager](acm-services.md)\.

 If you use AWS CloudFormation to quickly create and delete multiple test environments, we recommend that you do not create a separate ACM Certificate for each environment\. Doing so will quickly exhaust your certificate limit\. For more information, see [Limits](acm-limits.md)\. Instead, create a wildcard certificate that covers all of the domain names that you are using for testing\. For example, if you repeatedly create ACM Certificates for domain names that vary by only a version number, such as *<version>*`.service.example.com`, create instead a single wildcard certificate for *<\*>*`.service.example.com`\. Include the wildcard certificate in the template that AWS CloudFormation uses to create your test environment\. 

## Certificate Pinning<a name="best-practices-pinning"></a>

Certificate pinning, sometimes known as SSL pinning, is a process that you can use in your application to validate a remote host by associating that host directly with its X\.509 certificate or public key instead of with a certificate hierarchy\. The application therefore uses pinning to bypass SSL/TLS certificate chain validation\. The typical SSL validation process checks signatures throughout the certificate chain from the root certificate authority \(CA\) certificate through the subordinate CA certificates, if any\. It also checks the certificate for the remote host at the bottom of the hierarchy\. Your application can instead pin to the certificate for the remote host to say that *only that* certificate and not the root certificate or any other in the chain is trusted\. You can add the remote host's certificate or public key to your application during development\. Alternatively, the application can add the certificate or key when it first connects to the host\.

**Warning**  
We recommend that your application **not** pin an ACM Certificate\. ACM performs [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md) to automatically renew your Amazon\-issued SSL/TLS certificates before they expire\. To renew a certificate, ACM generates a new public\-private key pair\. If your application pins the ACM Certificate and the certificate is successfully renewed with a new public key, the application might be unable to connect to your domain\.

If you decide to pin a certificate, the following options will not hinder your application from connecting to your domain:

+ [Import your own certificate](http://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html) into ACM and then pin your application to the imported certificate\. ACM doesn't try to automatically renew imported certificates\.

+ Pin your application to an [ Amazon root certificate](https://www.amazontrust.com/repository/)\.

## Domain Validation<a name="best-practices-validating"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must verify that you own or control all the domains that you specified in your request\. You can perform verification using either email or DNS\. For more information, see [Use Email to Validate Domain Ownership](gs-acm-validate-dns.md) and [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

If you can change your DNS configuration, we recommend that you use DNS validation\. ACM sends up to 8 validation email messages for each domain in the request\. At least 1 of the messages must be acted upon within 72 hours\. For example, if you request a certificate with five domain names, you will receive up to 40 validation email messages\. However, if you use DNS validation, you need to create only five new DNS records\.

## Adding or Deleting Domain Names<a name="best-practices-add-delete"></a>

You cannot add or remove domain names from an existing ACM Certificate\. Instead you must request a new certificate with the revised list of domain names\. For example, if your certificate has five domain names and you want to add four more, you must request a new certificate with all nine domain names\. As with any new certificate, you must validate ownership of all the domain names in the request, including the names that you previously validated for the original certificate\. 

If you are using email validation, the amount of administrative work necessary to create the new certificate is problematic\. In the preceding example, if you have five domains and you add four more, you receive up to 72 email messages\. At least 9 of which must be acted upon within 72 hours\. As the number of domain names in the certificate request increases, so does the work required to validate ownership\. 

If you use DNS validation instead, you need to create only four new DNS records, one for each of the new domain names that you want to add to the ACM Certificate\. This assumes that you used DNS validation for the original five domains\. We recommend that, if possible, you use DNS validation\.

## Requesting a Limit Increase<a name="best-practices-limit-increase"></a>

If you use email validation, you receive up to 8 validation email messages for each domain, at least 1 of which must be acted upon within 72 hours\. For example, when you request a certificate with five domain names, you receive up to 40 validation messages, at least 5 of which must be acted upon within 72 hours\. As the number of domain names in the certificate request increases, so does the work required to use email to validate domain ownership\. 

If you use DNS validation instead, you must write one new DNS record to the database for the FQDN you want to validate\. ACM sends you the record to create and later queries the database to determine whether the record has been added\. Adding the record asserts that you own or control the domain\. In the preceding example, if you request a certificate with five domain names, you must create five DNS records\. We recommend that you use DNS validation when possible\. 