# \(Optional\) Configure Email for Your Domain<a name="setup-email"></a>

**Note**  
 The following steps are required only if you use email validation to assert that you own or control the FQDN \(fully qualified domain name\) specified in your certificate request\. ACM requires that you validate ownership or control before it issues a certificate\. You can use either email validation or DNS validation\. For more information about email validation, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\.   
If you are able to edit your DNS configuration, we recommend that you use DNS domain validation rather than email validation\. DNS validation removes the need to configure email for the domain name\. For more information about DNS validation, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

Use your registrar's website to associate your contact addresses with your domain name\. The registrar adds the contact email addresses to the WHOIS database and adds one or more mail servers to the mail exchanger \(MX\) records of a DNS server\. If you choose to use email validation, ACM sends email to the contact addresses and to five common administrative addresses formed from your MX record\. ACM sends up to eight validation email messages every time you create a new certificate, renew a certificate, or request new validation mail\. The validation email contains instructions for confirming that the domain owner or an appointed representative approves the ACM Certificate\. For more information, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you have trouble with validation email, see [Troubleshoot Email Validation Problems](troubleshooting-email-validation.md)\. 

## WHOIS Database<a name="setup-email-whois"></a>

The WHOIS database contains contact information for your domain\. To validate your identity, ACM sends an email to the following three addresses in WHOIS\. You must make sure that your contact information is public or that email that is sent to an obfuscated address is forwarded to your real email address\. 
+ Domain registrant
+ Technical contact
+ Administrative contact

## MX Record<a name="setup-email-mx"></a>

When you register your domain, your registrar sends your mail exchanger \(MX\) record to a Domain Name System \(DNS\) server\. An MX record indicates which servers accept mail for your domain\. The record contains a fully qualified domain name \(FQDN\)\. You can request a certificate for apex domains or subdomains\. 

For example, if you use the console to request a certificate for abc\.xyz\.example\.com, ACM first tries to find the MX record for that subdomain\. If that record cannot be found, ACM performs an MX lookup for xyz\.example\.com\. If that record cannot be found, ACM performs an MX lookup for example\.com\. If that record cannot be found or there is no MX record, ACM chooses the original domain for which the certificate was requested \(abc\.xyz\.example\.com in this example\)\. ACM then sends email to the following five common system administration addresses for the domain or subdomain: 
+ administrator@*your\_domain\_name*
+ hostmaster@*your\_domain\_name*
+ postmaster@*your\_domain\_name*
+ webmaster@*your\_domain\_name*
+ admin@*your\_domain\_name*

If you are using the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API operation or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command, AWS does not perform an MX lookup\. Instead, `RequestCertificate` lets you specify both your domain name and the name of a validation domain\. If you specify the optional `ValidationDomain` parameter, AWS sends the preceding five email messages there rather than to your domain\. 

ACM always sends validation email to the five common addresses listed previously whether you are using the console, the API, or the AWS CLI\. However, AWS performs an MX lookup only when you use the console to request a certificate\. 

If you do not receive validation email, see [Not Receiving Validation Email](troubleshooting-email-validation.md#troubleshooting-no-mail) for information about possible causes and workarounds\. 