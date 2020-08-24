# Troubleshoot Email Validation Problems<a name="troubleshooting-email-validation"></a>

Consult the following guidance if you are having trouble validating a certificate with email\.

**Topics**
+ [Not Receiving Validation Email](#troubleshooting-no-mail)
+ [Email Sent to Subdomain](#troubleshooting-email-subdomains)
+ [Hidden Contact Information](#troubleshooting-email-obfuscation)
+ [Certificate Renewals](#troubleshooting-email-renewals)
+ [WHOIS Throttling](#troubleshooting-email-whois-throttle)

## Not Receiving Validation Email<a name="troubleshooting-no-mail"></a>

When you request a certificate from ACM and choose email validation, domain validation email is sent to three contact addresses specified in WHOIS and to five common administrative addresses\. For more information, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you are experiencing problems receiving validation email, review the suggestions that follow\.

**Where to look for email**  
Validation email is sent to contact addresses listed in WHOIS and to common administrative addresses for the domain\. Email is not sent to the AWS account owner unless the owner is also listed as a domain contact in WHOIS\. Review the list of email addresses that are displayed in the ACM console \(or returned from the CLI or API\) to determine where you should be looking for validation email\. To see the list, click the icon next to the domain name in the box labeled **Validation not complete**\.

**The email is marked as spam**  
Check your spam folder for the validation email\.

**GMail automatically sorts your email**  
If you are using GMail, the validation email may have been automatically sorted into the **Updates** or **Promotions** tabs\.

**The domain registrar does not display contact information or privacy protection is enabled**  
In some cases, the domain registrant, technical, and administrative contacts in WHOIS may not be publicly available, and AWS therefore cannot reach these contacts\. At your discretion, you can choose to configure your registrar to list your email address in WHOIS, although not all registrars support this option\. You may be required to make a change directly at your domain's registry\. In other cases, the domain contact information may be using a privacy address, such as those provided through WhoisGuard or PrivacyGuard\.   
For domains purchased from Route 53, privacy protection is enabled by default and your email address is mapped to a `whoisprivacyservice.org` or `contact.gandi.net` email address\. Ensure that your registrant email address on file with your domain registrar is up to date so that the email sent to these obscured email addresses can be forwarded to an email address that you control\.   
Privacy protection for some domains that your purchase with Route 53 will be enabled even if you choose to make your contact information public\. For example, privacy protection for the \.ca top level domain cannot be programmatically disabled by Route 53\. You must contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/) and request that privacy protection be disabled\.
If email contact information for your domain is not available through WHOIS, or if email sent to the contact information does not reach the domain owner or an authorized representative, we recommend that you configure your domain or subdomain to receive email sent to one or more of the common administrative addresses formed by prepending admin@, administrator@, hostmaster@, webmaster@, and postmaster@ to the requested domain name\. For more information about configuring email for your domain, see the documentation for your email service provider and follow the instructions at [\(Optional\) Configure Email for Your Domain](setup-email.md)\. If you are using Amazon WorkMail, see [Working with Users](https://docs.aws.amazon.com/workmail/latest/adminguide/users_overview.html) in the Amazon WorkMail Administrator Guide\.  
After making available at least one of the eight email addresses to which AWS sends validation email and confirming that you can receive email for that address, you are ready to request a certificate through ACM\. After you make a certificate request, ensure the intended email address appears in the list of email addresses in the AWS Management Console\. While the certificate is in the **Pending validation** state, you can expand the list to view it by clicking the icon next to the domain name in the box labeled **Validation not complete**\. You can also view the list in **Step 3: Validate** of the ACM **Request a Certificate** wizard\. The listed email addresses are the ones to which email was sent\.

**Missing or incorrectly configured MX records**  
An MX record is a resource record in the Domain Name System \(DNS\) database that specifies one or more mail servers that accept email messages for your domain\. If your MX record is missing or misconfigured, email can not be sent to any of the five common system administration addresses specified at [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. Fix your missing or misconfigured MX record and try to resend the email or request your certificate again\.   
Currently, we recommend that you wait at least one hour before attempting to resend the email or requesting your certificate\.
To bypass requiring an MX record, you can use the `ValidationDomain` option in the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command to specify the domain name to which ACM sends validation emails\. If you use the API or the AWS CLI, AWS does not perform an MX lookup\. 

**Contact the Support Center**  
If, after reviewing the preceding guidance, you still don't receive the domain validation email, please visit the [AWS Support Center](https://console.aws.amazon.com/support/home) and create a case\. If you don't have a support agreement, post a message to the [ACM Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.

## Email Sent to Subdomain<a name="troubleshooting-email-subdomains"></a>

If you are using the console and request a certificate for a subdomain name such as `sub.test.example.com`, then ACM checks to see if there is an MX record for `sub.test.example.com`\. If not, then the parent domain `test.example.com` is checked, and so on, up to the base domain `example.com`\. If an MX record is found, the search stops and a validation email is sent to the common administration addresses for the subdomain\. So for example, if an MX record is found for `test.example.com`, email is sent to admin@test\.example\.com, administrator@test\.example\.com, and the other administrative addresses specified in [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If an MX record is not found in any of the subdomains, email is sent to the subdomain that you originally requested the certificate for\. For a thorough discussion of how to set up your email and how ACM works with DNS and the WHOIS database, see [\(Optional\) Configure Email for Your Domain](setup-email.md)\. 

Instead of using the console, you can use the `ValidationDomain` option in the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API or the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) AWS CLI command to specify the domain name to which ACM sends validation emails\. If you use the API or the AWS CLI, AWS does not perform an MX lookup\. 

## Hidden Contact Information<a name="troubleshooting-email-obfuscation"></a>

A common problem occurs when you attempt to create a new certificate\. Some registrars allow you to hide your contact information in your WHOIS listing\. Others allow you to substitute your real email address with a privacy \(or proxy\) address\. This prevents you from receiving validation email at your registered contact addresses\.

To receive mail, ensure that your contact information is public in WHOIS, or if your WHOIS listing shows a privacy email address, ensure that email sent to the privacy address is forwarded to your real email address\. After your WHOIS setup is complete and as long as your certificate request has not timed out, you can choose to resend the validation email\. ACM performs a new WHOIS/MX lookup and sends validation email to your now public contact address\.

## Certificate Renewals<a name="troubleshooting-email-renewals"></a>

If you made your WHOIS information public when you requested a new certificate and then later obfuscated your information, ACM cannot retrieve your registered contact addresses when you attempt to renew your certificate\. ACM sends validation email to these contact addresses and to five common administrative addresses formed by using your MX record\. To address this problem, make your WHOIS information public again and resend the validation emails\. ACM performs a new WHOIS/MX lookup and sends validation email to your now public contact addresses\.

## WHOIS Throttling<a name="troubleshooting-email-whois-throttle"></a>

Sometimes ACM is unable to contact the WHOIS server even after you have sent multiple requests for validation email\. This problem is external to AWS\. That is, AWS does not control the WHOIS servers and cannot prevent WHOIS server throttling\. If you experience this problem, create a case at the [ AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm) for help with a workaround\.