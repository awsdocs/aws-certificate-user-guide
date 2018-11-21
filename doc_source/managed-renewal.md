# Managed Renewal for ACM's Amazon\-Issued Certificates<a name="managed-renewal"></a>

ACM provides managed renewal for your Amazon\-issued SSL/TLS certificates\. This includes both public and private certificates issued by using ACM\. ACM tries to renew the certificates before they expire\. If possible, ACM renews your certificates automatically with no action required from you\. A certificate is eligible for renewal if it is associated with another AWS service, such as Elastic Load Balancing or CloudFront, or if it has been exported since being issued or last renewed\.

**Note**  
 Automatic renewal is not available for ACM Private CA certificates for which ACM does not create the private key and certificate signing request \(CSR\), such as certificates issued directly from your ACM Private CA without ACM certificate management\. Additionally, automatic renewal is not available for [imported certificates](import-certificate.md)\. For more information, see [How Manual Domain Validation Works](http://docs.aws.amazon.com/acm/latest/userguide/how-domain-validation-works.html#how-manual-domain-validation-works)\.

**Note**  
When ACM renews a certificate, the certificate's Amazon Resource Name \(ARN\) remains the same\. Also, ACM Certificates are [regional resources](acm-regions.md)\. If you have certificates for the same domain name in multiple AWS Regions, ACM renews each of these certificates independently\.

**Important**  
Your ACM Certificate must be actively associated with a supported AWS service before it can be automatically renewed\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

For more information about managed certificate renewal, see the following topics\. If you encounter problems with managed renewal, see [Troubleshoot Managed Certificate Renewal Problems](troubleshooting-renewal.md)\.

**Topics**
+ [How Domain Validation Works](how-domain-validation-works.md)
+ [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)
+ [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)