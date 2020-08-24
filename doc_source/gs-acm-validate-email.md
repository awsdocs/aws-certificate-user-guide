# Using Email to Validate Domain Ownership<a name="gs-acm-validate-email"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must verify that you own or control all of the domains that you specified in your request\. You can perform verification using either email or DNS\. This topic discusses email validation\. For information about DNS validation, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

If you encounter problems using email validation, see [Troubleshoot Email Validation Problems](troubleshooting-email-validation.md)\.

**Note**  
Validation applies only to public certificates issued by AWS Certificate Manager \(ACM\)\. ACM does not validate domain ownership for [imported certificates](import-certificate.md) or for certificates signed by a private CA\.

AWS Certificate Manager \(ACM\) sends email to the 3 contact addresses listed in WHOIS and to 5 common system addresses for each domain that you specify\. That is, up to 8 email messages will be sent for every domain name and subject alternative name that you include in your request\. For example, if you specify only 1 domain name, you will receive up to 8 email messages\. To validate, you must act on 1 of these 8 messages within 72 hours\. If you specify 3 domain names, you will receive up to 24 messages\. To validate, you must act on at least 3 of these emails, 1 for each name that you specified, within 72 hours\.

Email is sent to the following three registered contact addresses in WHOIS:
+ Domain registrant
+ Technical contact
+ Administrative contact

**Note**  
Some registrars allow you to hide your contact information in your WHOIS listing, and others allow you to substitute your real email address with a privacy \(or proxy\) address\. To prevent problems with receiving the domain validation email from ACM, ensure that your contact information is visible in WHOIS\. If your WHOIS listing shows a privacy email address, ensure that email sent to that address is forwarded to your real email address\. Or simply list your real email address instead\.

If you use the console to request a certificate, ACM performs an MX lookup to determine which servers accept email for your domain and sends mail to the following five common system addresses for first domain found\. If you use the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command, ACM does not perform an MX lookup\. Instead, it sends email to the domain name you specify in the `DomainName` parameter or in the optional `ValidationDomain` parameter\. For more information, see [MX Record](setup-email.md#setup-email-mx)\. 
+ administrator@*your\_domain\_name*
+ hostmaster@*your\_domain\_name*
+ postmaster@*your\_domain\_name*
+ webmaster@*your\_domain\_name*
+ admin@*your\_domain\_name*

For more information about how ACM determines the email addresses for your domains, see [\(Optional\) Configure Email for Your Domain](setup-email.md)\. 

The console shows where the validation email messages have been sent for the first domain name you specify in your request\. The email is sent from **no\-reply@certificates\.amazon\.com**\. 

![\[Console showing where validation emails were sent.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/PendingValidationEmail.png)

**Note**  
There is an exception to the process described above\. If you request an ACM certificate for a domain name that begins with **www** or a wild card asterisk \(**\***\), ACM removes the leading **www** or asterisk and sends email to the administrative addresses\. These addresses are formed by prepending admin@, administrator@, hostmaster@, postmaster@, and webmaster@ to the remaining portion of the domain name\. For example, if you request an ACM certificate for www\.example\.com, email is sent to admin@example\.com rather than to admin@www\.example\.com\. Likewise, if you request an ACM certificate for \*\.test\.example\.com, email is sent to admin@test\.example\.com\. The remaining common administrative addresses are similarly formed\.

**Note**  
Ensure that email is sent to the administrative addresses for an apex domain, such as `example.com`, rather than to the administrative addresses for a subdomain, such as `test.example.com`\. To do that, specify the `ValidationDomain` option in the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command\. This feature is not currently supported when you use the console to request a certificate\. 

The following example shows the validation email that is sent for every domain name that you specify in your certificate request\. 

![\[Validation letter with included validation number.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-validation-email.png)

Choose the link that sends you to the Amazon Certificate Approvals website and then choose **I Approve**\. 

![\[Approve your request for an ACM certificate.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-validation-website.png)

After choosing **I Approve**, a website opens to indicate that your request was successful\. 

![\[Success website.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-success-website.png)

You can navigate back to the ACM console by clicking a link on the success page\. It can take up to several hours for ACM to validate the domain name and issue the certificate\. During this time, ACM shows the validation status as **Pending validation**\. After validating the domain name, ACM changes the validation status to **Success**\. After AWS issues the certificate, ACM changes the certificate status to **Issued**\. 

![\[ACM shows that your request was successful.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-success-validation-console.png)