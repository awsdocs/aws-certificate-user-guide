# Managed Renewal for ACM's Amazon\-Issued Certificates<a name="managed-renewal"></a>

ACM provides managed renewal for your Amazon\-issued SSL/TLS certificates\. This means that ACM tries to renew the certificates before they expire\. If possible, ACM renews your certificates automatically with no action required from you\.

**Note**  
 Automatic renewal is not available for either [imported certificates](import-certificate.md) or for certificates associated with Route 53 private hosted zones\. You must renew these manually\. For more information, see [ How Manual Domain Validation Works ](http://docs.aws.amazon.com/acm/latest/userguide/how-domain-validation-works.html#how-manual-domain-validation-works)\.

**Note**  
When ACM renews a certificate, the certificate's Amazon Resource Name \(ARN\) remains the same\. Also, ACM Certificates are [regional resources](acm-regions.md)\. If you have certificates for the same domain name in multiple AWS Regions, ACM renews each of these certificates independently\.

**Important**  
Your ACM Certificate must be actively associated with a supported AWS service before it can be automatically renewed\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

For more information about managed certificate renewal, see the following topics\. If you encounter problems with managed renewal, see [Troubleshoot Managed Certificate Renewal Problems](troubleshooting-renewal.md)\.

**Topics**
+ [How Domain Validation Works](how-domain-validation-works.md)
+ [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)
+ [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)