# Renewal for domains validated by email<a name="email-renewal-validation"></a>

Managed renewal for an ACM certificate that was [email\-validated](email-validation.md) results in an email message giving notice that expiration is approaching\. Completing the renewal process requires a manual or programmatic response from the domain owner\. For information about programming a parser to respond to these messages, see [Automating email validation ](email-automation.md)\.

To re\-validate a domain that was originally validated by email, ACM periodically attempts to connect to it over HTTPS and examine the TLS certificate that is returned\. For domains that start with `www.`, ACM also sends HTTPS requests to the parent domain\. For example, if your domain is `www.example.com`, ACM sends periodic requests to `www.example.com` and to `example.com`\. For domains that don't start with `www.`, ACM also sends HTTPS requests to `www.domain`\. ACM treats wildcard domain names \(for example, `*.example.com`\) the same as the parent domain\. For examples, see the following table\. 


**Example domain names that ACM tries to reach for email validation**  

|  Domain name in the certificate  |  Domain names that ACM tries for validation  | 
| --- | --- | 
|  example\.com  |  example\.com www\.example\.com  | 
|  www\.example\.com  |  www\.example\.com example\.com  | 
|  \*\.example\.com  |  example\.com www\.example\.com  | 
|  subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 
|  www\.subdomain\.example\.com  |  www\.subdomain\.example\.com subdomain\.example\.com  | 
|  \*\.subdomain\.example\.com  |  subdomain\.example\.com www\.subdomain\.example\.com  | 

For each of domain names tried, email messages are sent to the eight addresses described in [Option 2: Email validation](email-validation.md)\.

## Request a domain validation email message for certificate renewal<a name="request-domain-validation-email-for-renewal"></a>

After you have configured contact email addresses for your domain \(see [\(Optional\) Configure email for your domain](setup-email.md)\), you can use the AWS Certificate Manager console or the ACM API to request that ACM send you a domain validation email for your certificate renewal\. You should do this in the following circumstances: 
+ You used email validation when initially requesting your ACM certificate\.
+ Your certificate's renewal status is **pending validation**\. For information about determining a certificate's renewal status, see [Check a certificate's renewal status](check-certificate-renewal-status.md)\.
+ You didn't receive or can't find the original domain validation email message that ACM sent for certificate renewal\.

**To request that ACM resend the domain validation email message \(console\)**

1. Open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Select the check box next to the certificate that requires manual domain validation\. Then choose **Actions**, **Resend validation email**\.

**To request that ACM resend the domain validation email \(ACM API\)**  
Use the [ResendValidationEmail](https://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html) operation in the ACM API\. In doing so, pass the ARN of the certificate, the domain that requires manual validation, and domain where you want to receive the domain validation emails\. The following example shows how to do this with the AWS CLI\. This example contains line breaks to make it easier to read\.

```
$ aws acm resend-validation-email --certificate-arn arn:aws:acm:us-east-2:111122223333:certificate/97b4deb6-8983-4e39-918e-ef1378924e1e
                                  --domain subdomain.example.com
                                  --validation-domain example.com
```