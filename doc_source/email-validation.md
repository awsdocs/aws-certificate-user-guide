# Option 2: Email validation<a name="email-validation"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must verify that you own or control all of the domains that you specified in your request\. You can perform verification using either email or DNS\. This topic discusses email validation\. For information about DNS validation, see [Option 1: DNS validationDNS validation](dns-validation.md)\. 

An email\-validated certificate is renewable only up to 825 days after its original validation date\. After 825 days, the domain owner or an authorized representative must re\-validate the domain ownership in order to renew the certificate\.

If you encounter problems using email validation, see [Troubleshoot email validation problems](troubleshooting-email-validation.md)\.

**Note**  
Validation applies only to publicly trusted certificates issued by ACM\. ACM does not validate domain ownership for [imported certificates](import-certificate.md) or for certificates signed by a private CA\.

 ACM sends email messages to the three contact addresses listed in the WHOIS database and to five common system addresses for each domain that you specify\. That is, up to eight email messages will be sent for every domain name and subject alternative name that you include in your request\. For example, if you specify only one domain name, you will receive up to eight email messages\. To validate, you must act on one of these eight messages within 72 hours\. If you specify three domain names, you will receive up to 24 messages\. To validate, you must act on at least three of these emails, one for each name that you specified, within 72 hours\.

Email messages are sent to the following three registered contact addresses in WHOIS:
+ Domain registrant
+ Technical contact
+ Administrative contact

**Note**  
Some registrars allow you to hide your contact information in your WHOIS listing, and others allow you to substitute your real email address with a privacy \(or proxy\) address\. To prevent problems with receiving the domain validation email from ACM, ensure that your contact information is visible in WHOIS\. If your WHOIS listing shows a privacy email address, ensure that email sent to that address is forwarded to your real email address\. Or simply list your real email address instead\.

If you use the console to request a certificate, ACM performs an MX lookup to determine which servers accept email for your domain and sends mail to the following five common system addresses for first domain found\. If you use the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command, ACM does not perform an MX lookup\. Instead, it sends email to the domain name you specify in the `DomainName` parameter or in the optional `ValidationDomain` parameter\. For more information, see [MX record](setup-email.md#setup-email-mx)\. 
+ administrator@*your\_domain\_name*
+ hostmaster@*your\_domain\_name*
+ postmaster@*your\_domain\_name*
+ webmaster@*your\_domain\_name*
+ admin@*your\_domain\_name*

For more information about how ACM determines the email addresses for your domains, see [\(Optional\) Configure email for your domain](setup-email.md)\. 

**Note**  
There is an exception to the process described above\. If you request an ACM certificate for a domain name that begins with **www** or a wild card asterisk \(**\***\), ACM removes the leading **www** or asterisk and sends email to the administrative addresses\. These addresses are formed by prepending admin@, administrator@, hostmaster@, postmaster@, and webmaster@ to the remaining portion of the domain name\. For example, if you request an ACM certificate for www\.example\.com, email is sent to admin@example\.com rather than to admin@www\.example\.com\. Likewise, if you request an ACM certificate for \*\.test\.example\.com, email is sent to admin@test\.example\.com\. The remaining common administrative addresses are similarly formed\.

**Note**  
Ensure that email is sent to the administrative addresses for an apex domain, such as `example.com`, rather than to the administrative addresses for a subdomain, such as `test.example.com`\. To do that, specify the `ValidationDomain` option in the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command\. This feature is not currently supported when you use the console to request a certificate\.  
Even when all messages are sent to a single email address, you must respond to one message for each domain or subdomain in order to validate it and generate the certificate\.

## \(Optional\) Resending validation mail<a name="gs-acm-resend"></a>

You can use email to validate that you own or control a domain\. Each email contains a validation token that you can use to approve a certificate request\. However, because the validation email required for the approval process can be blocked by spam filters or lost in transit, the validation token automatically expires after 72 hours\. If you do not receive the original email or the token has expired, you can request that the email be resent\. 

For persistent problems with email validation, see the [Troubleshoot email validation problems](troubleshooting-email-validation.md) section in [Troubleshooting](troubleshooting.md)\.

**Note**  
The following information applies only to certificates provided by ACM and only to certificates that use email validation\. Validation email is not required for [certificates that you imported into ACM](import-certificate.md)\. For information about DNS domain validation, see [Option 1: DNS validationDNS validation](dns-validation.md)\. 

**To resend validation email using the console**

Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. For a listed certificate showing a status of **Pending validation**, select its check box, choose **Actions**, and then choose **Resend validation email**\. If the 72\-hour period has passed and the certificate status has changed to **Timed out**, you cannot resend validation email\. 

**To resend validation email using the AWS CLI**

You can use the [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command to resend email\. 

```
aws acm resend-validation-email --certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012 --domain www.example.com --validation-domain example.com
```

**Note**  
The [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command applies only to ACM certificates for which you are using email validation\. Validation is not required for certificates that you have imported into ACM or for private certificates that you manage using ACM\.