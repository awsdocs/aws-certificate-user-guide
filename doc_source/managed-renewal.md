# Managed Renewal for ACM's Amazon\-Issued Certificates<a name="managed-renewal"></a>

ACM attempts to automatically renew your ACM Certificates before they expire\.

**Note**  
 Automatic renewal is not available for either imported certificates or for certificates associated with Amazon Route 53 private hosted zones\. You must renew these manually\. For more information, see [When Automatic Validation Fails](how-domain-validation-works.md#how-manual-domain-validation-works)\. 

**Note**  
When ACM renews a certificate, the certificate's Amazon Resource Name \(ARN\) remains the same\. Also, ACM Certificates are regional resources\. If you have certificates for the same domain name in multiple AWS Regions, ACM renews each of these certificates independently\.

**Important**  
Your ACM Certificate must be actively associated with a supported AWS service before it can be automatically renewed\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

For more information about managed certificate renewal, see the following topics\. If you encounter problems with managed renewal, see [Troubleshoot Managed Certificate Renewal Problems](troubleshooting-renewal.md)\.


+ [Automatic Domain Validation](how-domain-validation-works.md)
+ [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)
+ [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)