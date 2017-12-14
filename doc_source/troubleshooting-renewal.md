# Troubleshoot Managed Certificate Renewal Problems<a name="troubleshooting-renewal"></a>

ACM tries to automatically renew your ACM Certificates before they expire so that no action is required from you\. Consult the following topics if you have trouble with [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md)\. 


+ [Automatic Domain Validation](#troubleshooting-renewal-domain-validation)
+ [Asynchronous Process](#troubleshooting-renewal-domain-async)

## Automatic Domain Validation<a name="troubleshooting-renewal-domain-validation"></a>

Before ACM can renew your certificates automatically, the following must be true:

+ ACM must be able to establish an HTTPS connection with each domain in the certificate\.

+ For each connection, the certificate that is returned must match the one that ACM is renewing\.

+ Your certificate must be associated with an AWS service that is integrated with ACM\.

+ ACM must be able to validate each domain name listed in your certificate\.

To increase the likelihood that ACM can renew your certificate automatically, do the following: 

**Use the certificate with an AWS resource**  
Make sure that your certificate is in use with a supported AWS resource\. For information about the resources that ACM supports, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

**Configure the resource to accept HTTPS requests from the Internet**  
Make sure that the AWS resource that has your ACM Certificate is configured to accept HTTPS requests from the internet\.

**Configure DNS to route your domain name to the resource that hosts your ACM Certificate**  
Make sure that HTTPS requests to the domain names in your certificate are routed to the resource that has your certificate\.

## Asynchronous Process<a name="troubleshooting-renewal-domain-async"></a>

[Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md) is an asynchronous process\. This means that the steps don't occur in immediate succession\. After all domain names in an ACM Certificate have been validated, there might be a delay before ACM obtains the new certificate\. An additional delay can occur between the time when ACM obtains the renewed certificate and the time when that certificate is deployed to the AWS resources that use it\. Therefore, changes to the certificate status can take up to several hours to appear in the console\. 