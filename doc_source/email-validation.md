# Email validation<a name="email-validation"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must verify that you own or control all of the domains that you specified in your request\. You can perform verification using either email or DNS\. This topic discusses email validation\. For information about DNS validation, see [DNS validationDNS validation](dns-validation.md)\. 

**Note**  
 You need a working email address registered in your domain in order to use email validation\. Procedures for setting up an email address are outside the scope of this guide\.

ACM certificates are valid for 13 months \(395 days\)\. To be renewed, email\-validated certificates require an action by the domain owner\. ACM begins sending renewal notices 45 days before expiration, using the domain's WHOIS mailbox addresses and to five common administrator addresses\. The notifications contain a link that the domain owner can click for easy renewal\. Once all listed domains are validated, ACM issues a renewed certificate with the same ARN\.

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

If you use the console to request a certificate, ACM performs an MX lookup to determine which servers accept email for your domain and sends mail to the following five common system addresses for first domain found\. If you use the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command, ACM does not perform an MX lookup\. Instead, it sends email to the domain name that you specify in the `DomainName` parameter or in the optional `ValidationDomain` parameter\. For more information, see [MX record](setup-email.md#setup-email-mx)\. 
+ administrator@*your\_domain\_name*
+ hostmaster@*your\_domain\_name*
+ postmaster@*your\_domain\_name*
+ webmaster@*your\_domain\_name*
+ admin@*your\_domain\_name*

For more information about how ACM determines the email addresses for your domains, see [\(Optional\) Configure email for your domain](setup-email.md)\. 

**Exception to this process**  
If you request an ACM certificate for a domain name that begins with **www** or a wild\-card asterisk \(**\***\), ACM removes the leading **www** or asterisk and sends email to the administrative addresses\. These addresses are formed by prepending admin@, administrator@, hostmaster@, postmaster@, and webmaster@ to the remaining portion of the domain name\. For example, if you request an ACM certificate for www\.example\.com, email is sent to admin@example\.com rather than to admin@www\.example\.com\. Likewise, if you request an ACM certificate for \*\.test\.example\.com, email is sent to admin@test\.example\.com\. The remaining common administrative addresses are similarly formed\.

**Note**  
Ensure that email is sent to the administrative addresses for an apex domain, such as `example.com`, rather than to the administrative addresses for a subdomain, such as `test.example.com`\. To do that, specify the `ValidationDomain` option in the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command\. This feature is not currently supported when you use the console to request a certificate\.  
Even when all messages are sent to a single email address, you must respond to one message for each domain or subdomain in order to validate it and generate the certificate\.

## Certificate expiration and renewal<a name="renewal"></a>

ACM certificates are valid for 13 months \(395 days\)\. To be renewed, email\-validated certificates require an action by the domain owner\. ACM begins sending renewal notices 45 days before expiration\. These notices go to the domain's WHOIS mailbox addresses and to five common administrator addresses\. The notifications contain a link that the domain owner can click for easy renewal\. Once all listed domains are validated, ACM issues a renewed certificate with the same ARN\.

## \(Optional\) Resending validation mail<a name="gs-acm-resend"></a>

Each validation email contains a token that you can use to approve a certificate request\. However, because the validation email required for the approval process can be blocked by spam filters or lost in transit, the token automatically expires after 72 hours\. If you do not receive the original email or the token has expired, you can request that the email be resent\. 

For persistent problems with email validation, see the [Troubleshoot email validation problems](troubleshooting-email-validation.md) section in [Troubleshooting](troubleshooting.md)\.

**Note**  
The following information applies only to certificates provided by ACM and only to certificates that use email validation\. Validation email is not required for private PKI certificates or for [certificates that you imported into ACM](import-certificate.md)\. For information about DNS domain validation, see [DNS validationDNS validation](dns-validation.md)\. 

**To resend validation email using the console**

1. Sign in to the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. In the list of certificates, choose the **Certificate ID** of the certificate you want to validate\. This action opens a details page\.
**Note**  
Depending on how you have ordered the list, a certificate you are looking for might not be immediately visible\. You can click the black triangle at right to change the ordering\. You can also navigate through multiple pages of certificates using the page numbers at upper\-right\.

1. In the **Domains** section, choose **Resend validation email**, select each of the domains requiring validation, and then choose **Resend**\. The **Successfully resent validation emails** banner should appear\.

**To resend validation email using the AWS CLI**

You can use the [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command to resend email\. 

```
$ aws acm resend-validation-email --certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012 --domain www.example.com --validation-domain example.com
```

**Note**  
The [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command applies only to ACM certificates for which you are using email validation\. Validation is not required for certificates that you have imported into ACM or for private certificates that you manage using ACM\.