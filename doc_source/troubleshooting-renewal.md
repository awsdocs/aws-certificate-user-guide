# Troubleshoot Managed Certificate Renewal Problems<a name="troubleshooting-renewal"></a>

ACM tries to automatically renew your ACM certificates before they expire so that no action is required from you\. Consult the following topics if you have trouble with [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md)\. 

**Topics**
+ [Preparing for Automatic Domain Validation](#troubleshooting-renewal-domain-validation)
+ [Handling Failures in Managed Certificate Renewal](#troubleshooting-automatic-renewal)
+ [Understanding Renewal Timing](#troubleshooting-renewal-domain-async)

## Preparing for Automatic Domain Validation<a name="troubleshooting-renewal-domain-validation"></a>

Before ACM can renew your certificates automatically, the following must be true:
+ ACM must be able to establish an HTTPS connection with each domain in the certificate\.
+ For each connection, the certificate that is returned must match the one that ACM is renewing\.
+ Your certificate must be associated with an AWS service that is integrated with ACM\.
+ ACM must be able to validate each domain name listed in your certificate\.

To increase the likelihood that ACM can renew your certificate automatically, do the following: 

**Use the certificate with an AWS resource**  
Make sure that your certificate is in use with a supported AWS resource\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

**Configure the resource to accept HTTPS requests from the internet**  
Make sure that the AWS resource that has your ACM certificate is [configured to accepts HTTPS requests from the internet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html)\.

**Configure DNS to route your domain name to the resource that hosts your ACM certificate**  
Make sure that HTTPS requests to the domain names in your certificate are routed to the resource that has your certificate\.

## Handling Failures in Managed Certificate Renewal<a name="troubleshooting-automatic-renewal"></a>

When a certificate is 60 days away from expiration, ACM automatically attempts to renew it every hour\. If ACM is unable to renew the certificate after 15 days, you will receive an email with further instructions on how to manually fix the renewal problem\. This process differs depending on how the certificate was originally validated\.

### Managed Certificate Renewal for Email\-Validated Certificates<a name="troubleshooting-renewal-email-validation-failure"></a>

If ACM fails to renew a certificate you originally validated by email, ACM sends an email for each domain name remaining in the `PENDING_VALIDATION` state\. The domain owner or an authorized representative of the domain owner must take action to validate each domain name that failed validation\. See [Validate with Email](gs-acm-validate-email.md) for instructions on identifying which domains are in the `PENDING_VALIDATION` state and repeating the validation process for those domains\.

### Managed Certificate Renewal for DNS\-Validated Certificates<a name="troubleshooting-renewal-domain-validation-failure"></a>

If ACM fails to renew a certificate you originally validated with DNS validation, it is most likely due to missing or inaccurate CNAME records in your DNS configuration\. If this occurs, ACM notifies you that the certificate could not be renewed automatically\. You must insert the correct CNAME records into your DNS database\. You can find the CNAME records for your domains by expanding your certificate and its domain entries in the ACM console\. Refer to the figures below for details\. You can also retrieve CNAME records by using the [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) operation in the ACM API or the [describe\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) command in the ACM CLI\. For more information, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\.

![\[Select the target certificate from the console.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/Dns-renewal-1.png)

![\[Expand the certificate window to find the certificate's CNAME information.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/Dns-renewal-2.png)

If the problem persists, contact the [Support Center](https://console.aws.amazon.com/support)\.

## Understanding Renewal Timing<a name="troubleshooting-renewal-domain-async"></a>

[Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md) is an asynchronous process\. This means that the steps don't occur in immediate succession\. After all domain names in an ACM certificate have been validated, there might be a delay before ACM obtains the new certificate\. An additional delay can occur between the time when ACM obtains the renewed certificate and the time when that certificate is deployed to the AWS resources that use it\. Therefore, changes to the certificate status can take up to several hours to appear in the console\. 