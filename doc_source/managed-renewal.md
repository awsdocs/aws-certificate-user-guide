# Managed Renewal for ACM's Amazon\-Issued Certificates<a name="managed-renewal"></a>

ACM provides managed renewal for your Amazon\-issued SSL/TLS certificates\. This includes both public and private certificates issued by using ACM\. If possible, ACM renews your certificates automatically with no action required from you\. A certificate is eligible for automatic renewal subject to the following considerations:
+ ELIGIBLE if associated with another AWS service, such as Elastic Load Balancing or CloudFront\.
+ ELIGIBLE if exported since being issued or last renewed\.
+ ELIGIBLE if it is a private certificate issued by calling the ACM [https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API\.
+ ELIGIBLE if it is a private certificate issued through the [management console](gs-acm-request-private.md)\.
+ NOT ELIGIBLE if it is a private certificate issued by calling the ACM Private CA [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_IssueCertificate.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_IssueCertificate.html) API\.
+ NOT ELIGIBLE if [imported](import-certificate.md)\.

When ACM renews a certificate, the certificate's Amazon Resource Name \(ARN\) remains the same\. Also, ACM certificates are [regional resources](acm-regions.md)\. If you have certificates for the same domain name in multiple AWS Regions, ACM renews each of these certificates independently\.

**Important**  
Your ACM certificate must be actively associated with a supported AWS service before it can be automatically renewed\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

For more information about managed certificate renewal, see the following topics\. If you encounter renewal problems, see [Troubleshooting Managed Certificate Renewal](troubleshooting-renewal.md)\.

**Note**  
If you encounter error messages when creating or renewing ACM Private CA certificates through ACM, consult the Troubleshooting section [Private Certificate Exception Handling](exceptions.md#private_certificate_exception_handling)\.

**Topics**
+ [How Domain Validation Works](how-domain-validation-works.md)
+ [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)
+ [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)