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

For each of domain names tried, email messages are sent to the eight addresses described in [Email validation](email-validation.md)\.