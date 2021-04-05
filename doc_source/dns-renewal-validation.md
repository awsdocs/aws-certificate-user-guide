# Renewal for domains validated by DNS<a name="dns-renewal-validation"></a>

Managed renewal is fully automated for ACM certificates that were originally issued using [DNS validation](dns-validation.md)\.

At 60 days prior to expiration, ACM checks for the renewal criteria:
+ The certificate is currently in use by an AWS service\.
+ A valid DNS record for the apex domain exists\.
+ The required CNAME token is present and accessible in the DNS record\.
+ Each domain and subdomain that is named in the certificate is present in the DNS record\.

If all of these criteria are met, ACM considers the domain names validated and renews the certificate\. 

If ACM cannot automatically validate a domain name, it notifies the domain owner that manual action is needed to validate the domain and complete certificate renewal\. These notifications are sent at 45 days, 30 days, seven days, and one day prior to expiration\. The most common reason for automatic validation to fail is that the required CNAME has been inadvertently changed or removed\.