# How Domain Validation Works<a name="how-domain-validation-works"></a>

Before renewing a certificate, ACM tries to automatically validate each domain name in the certificate\. If ACM can't automatically validate a domain name, it notifies the domain owner that they need to take action to manually validate it and complete the certificate renewal\. If the certificate is in use \(associated with an AWS service that is integrated with ACM\) and if all of the domain names in the certificate can be validated, ACM renews the certificate\. 

## Understanding Automatic Domain Validation<a name="how-automatic-domain-validation-works"></a>

To validate a domain, ACM sends automated, periodic HTTPS requests to it\. For domains that start with `www.`, ACM also sends HTTPS requests to the parent domain\. For example, if your domain is `www.example.com`, ACM sends periodic requests to `www.example.com` and to `example.com`\. For domains that don't start with `www.`, ACM also sends HTTPS requests to `www.domain`\. ACM treats wildcard domain names \(for example, `*.example.com`\) the same as the parent domain\. For examples, see the following table\. 

**Note**  
If any HTTPS connection attempt is successful, ACM attempts to renew the certificate automatically\. Automatic renewal will not result in an email notification even if the certificate was originally validated by email\.


**Example domain names that ACM uses for automatic validation**  

|  Domain name in the certificate  |  Domain names that ACM uses for automatic validation  | 
| --- | --- | 
|  example\.com  |  example\.com www\.example\.com  | 
|  www\.example\.com  |  www\.example\.com example\.com  | 
|  \*\.example\.com  |  example\.com www\.example\.com  | 
|  subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 
|  www\.subdomain\.example\.com  |  www\.subdomain\.example\.com subdomain\.example\.com  | 
|  \*\.subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 

If ACM successfully establishes an HTTPS connection, ACM examines the certificate that is returned to ensure it matches the one that ACM is renewing\. If the certificate matches, ACM considers the domain name validated\. 

## When Automatic Certificate Renewal Fails<a name="how-manual-domain-validation-works"></a>

If ACM is unable to automatically validate one or more domain names in a certificate, ACM notifies the domain owner that action must be taken to manually validate the domain\. A domain can require manual validation for the following reasons: 
+ ACM can't establish an HTTPS connection with the domain\.
+ The certificate that is returned in the response to the HTTPS requests doesn't match the one that ACM is renewing\.

When a certificate is 45 days from expiration and one or more domain names in the certificate requires manual validation, ACM notifies the domain owner in the following ways\. 

**For email\-validated certificates:**  
If the certificate was last validated by email, ACM sends an email for each domain name that requires manual validation to the domain owner\. To ensure that this email can be received, the domain owner must correctly configure email for each domain\. For more information, see [\(Optional\) Configure Email for Your Domain](setup-email.md)\. The email contains a link that performs the validation\. This link expires after 72 hours\. If necessary, you can use the AWS Certificate Manager console, AWS CLI, or API to request that ACM resend the domain validation email\. For more information, see [Request a Domain Validation Email for Certificate Renewal](request-domain-validation-email-for-renewal.md)\.   
Email\-validated certificates are automatically renewed up to 825 days after their last manual validation date\. After 825 days, the domain owner or an authorized representative must manually re\-validate ownership of the domain in order to proceed with the renewal\. In order to avoid this issue, we recommend creating a new certificate and using DNS validation if you are able to do so, as DNS\-validated certificates are re\-validated indefinitely as long as they are properly configured\.

**By notification in your AWS Personal Health Dashboard**  
ACM sends notifications to your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home#/dashboard/scheduled-changes) to let you know that one or more domain names in the certificate require validation before the certificate can be renewed\. ACM sends these notifications when your certificate is 45 days, 30 days, 15 days, 7 days, 3 days, and 1 day from expiration\. These notifications are informational only\.