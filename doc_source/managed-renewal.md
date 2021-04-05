# Managed renewal for ACM certificates<a name="managed-renewal"></a>

ACM provides managed renewal for your Amazon\-issued SSL/TLS certificates\. This means that ACM will either renew your certificates automatically \(if you are using DNS validation\), or it will send you email notices when expiration is approaching\. These services are provided for both public and private ACM certificates\.

A certificate is eligible for automatic renewal subject to the following considerations:
+ ELIGIBLE if associated with another AWS service, such as Elastic Load Balancing or CloudFront\.
+ ELIGIBLE if exported since being issued or last renewed\.
+ ELIGIBLE if it is a private certificate issued by calling the ACM [https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API *and* then exported or associated with another AWS service\.
+ ELIGIBLE if it is a private certificate issued through the [management console](gs-acm-request-private.md) *and* then exported or associated with another AWS service\.
+ NOT ELIGIBLE if it is a private certificate issued by calling the ACM Private CA [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_IssueCertificate.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_IssueCertificate.html) API\.
+ NOT ELIGIBLE if [imported](import-certificate.md)\.
+ NOT ELIGIBLE if already expired\.

When ACM renews a certificate, the certificate's Amazon Resource Name \(ARN\) remains the same\. Also, ACM certificates are [regional resources](acm-regions.md)\. If you have certificates for the same domain name in multiple AWS Regions, each of these certificates must be renewed independently\.

**Topics**
+ [Renewal for domains validated by DNS](dns-renewal-validation.md)
+ [Renewal for domains validated by email](email-renewal-validation.md)
+ [Renewing certificates in a private PKI](renew-private-cert.md)
+ [Check a certificate's renewal status](check-certificate-renewal-status.md)
+ [Testing the managed renewal of your private PKI certificates](manual-renewal.md)