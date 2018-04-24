# How Domain Validation Works<a name="how-domain-validation-works"></a>

 Before renewing a certificate, ACM tries to automatically validate each domain name in the certificate\. For more information, see [How Automatic Domain Validation Works](#how-automatic-domain-validation-works)\. If ACM can't automatically validate a domain name, it notifies you that you need to take action to manually validate it\. For more information, see [When Automatic Validation Fails](#how-manual-domain-validation-works)\. If the certificate is in use \(associated with an AWS service that is integrated with ACM\) and if all of the domain names in the certificate can be validated, ACM renews the certificate\. 

**Topics**
+ [How Automatic Domain Validation Works](#how-automatic-domain-validation-works)
+ [When Automatic Validation Fails](#how-manual-domain-validation-works)

## How Automatic Domain Validation Works<a name="how-automatic-domain-validation-works"></a>

To validate a domain, ACM sends automated, periodic HTTPS requests to it\. For domains that start with `www.`, ACM also sends HTTPS requests to the parent domain\. For example, if your domain is `www.example.com`, ACM sends periodic requests to `www.example.com` and to `example.com`\. For domains that don't start with `www.`, ACM also sends HTTPS requests to `www.domain`\. ACM treats wildcard domain names \(for example, `*.example.com`\) the same as the parent domain\. For examples, see the following table\. 

**Note**  
If any HTTPS connection attempt is successful, ACM attempts to renew the certificate automatically\.


**Example domain names that ACM uses for automatic validation**  

|  Domain name in the certificate  |  Domain names that ACM use for automatic validation  | 
| --- | --- | 
|  example\.com  |  example\.com www\.example\.com  | 
|  www\.example\.com  |  www\.example\.com example\.com  | 
|  \*\.example\.com  |  example\.com www\.example\.com  | 
|  subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 
|  www\.subdomain\.example\.com  |  www\.subdomain\.example\.com subdomain\.example\.com  | 
|  \*\.subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 

If ACM successfully establishes an HTTPS connection, ACM examines the certificate that is returned to ensure it matches the one that ACM is renewing\. If the certificate matches, ACM considers the domain name validated\. 

## When Automatic Validation Fails<a name="how-manual-domain-validation-works"></a>

If ACM is unable to automatically validate one or more domain names in a certificate, ACM notifies you that you need to take action to manually validate the domain\. A domain can require manual validation for the following reasons: 
+ ACM can't establish an HTTPS connection with the domain\.
+ The certificate that is returned in the response to the HTTPS requests doesn't match the one that ACM is renewing\.

When your certificate is 45 days from expiration and one or more domain names in the certificate requires manual validation, ACM notifies you in the following ways: 

**By email to the domain owner \(email validation\)**  
If you originally used email validation when you requested the certificate, ACM sends email to the domain owner for each domain name that requires manual validation\. To ensure that this email can be received, the domain owner must correctly configure email for each domain\. For more information, see [\(Optional\) Configure Email for Your Domain](setup-email.md)\. The email contains a link that you can follow to perform the validation\. This link expires after 72 hours\. If necessary, you can use the AWS Certificate Manager console, AWS CLI, or API to request that ACM resend the domain validation email\. For more information, see [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)\. 

** By email to your AWS account \(DNS validation\)**  
If you originally used DNS validation when you requested your certificate, ACM sends email to the address associated with your AWS account\. The email informs you that ACM encountered a problem when attempting to renew your certificate\. The most likely problems are that the original CNAME record is no longer in place or that your certificate is not associated with an AWS service that is integrated with ACM\. If you want to validate your domain and renew your certificate, you must edit your DNS configuration to ensure that the original CNAME record is in place\. In addition, and you must make sure that your ACM Certificate is in use\. For more information about DNS validation, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**By notification in your AWS Personal Health Dashboard**  
ACM sends notifications to your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home#/dashboard/scheduled-changes) to let you know that one or more domain names in the certificate require renewal\. ACM sends these notifications when your certificate is 45 days, 30 days, 15 days, 7 days, 3 days, and 1 day from expiration\. These notifications are informational only\.