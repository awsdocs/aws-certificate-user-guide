# Certificate Request Fails<a name="troubleshooting-failed"></a>

If your request fails ACM and you receive one of the following error messages, follow the suggested steps to fix the problem\. You cannot resubmit a failed certificate request – after resolving the problem, submit a new request\.

**Topics**
+ [Error Message: No Available Contacts](#failed-no-available-contacts)
+ [Error Message: Domain Not Allowed](#failed-domain-not-allowed)
+ [Error Message: Additional Verification Required](#failed-additional-verification-required)
+ [Error Message: Invalid Public Domain](#failed-invalid-domain)
+ [Error Message: Other](#failed-other)

## Error Message: No Available Contacts<a name="failed-no-available-contacts"></a>

You chose email validation when requesting a certificate, but ACM could not find an email address to use for validating one or more of the domain names in the request\. To correct this problem, you can do one of the following:
+ Ensure that you have a working email address that is registered in WHOIS and that the address is visible when performing a standard WHOIS lookup for the domain names in the certificate request\. Typically, you do this through your domain registrar\.
+ Ensure your domain is configured to receive email\. Your domain's name server must have a mail exchanger record \(MX record\) so ACM's email servers know where to send the [domain validation email](gs-acm-validate-email.md)\.

Accomplishing just one of the preceding tasks is enough to correct this problem; you don't need to do both\. After you correct the problem, request a new certificate\. 

For more information about how to ensure that you receive domain validation emails from ACM, see [\(Optional\) Configure Email for Your Domain](setup-email.md) or [Not Receiving Validation Email](troubleshooting-email-validation.md#troubleshooting-no-mail)\. If you follow these steps and continue to get the **No Available Contacts** message, then [report this to AWS](https://console.aws.amazon.com/support/home) so that we can investigate it\.

## Error Message: Domain Not Allowed<a name="failed-domain-not-allowed"></a>

One or more of the domain names in the certificate request was reported as an unsafe domain by [VirusTotal](https://www.virustotal.com/gui/home/url)\. To correct the problem, try the following:
+ Search for your domain name on the [VirusTotal](https://www.virustotal.com/gui/home/url) website\. If your domain is reported as suspicious, see [Google Help for Hacked Websites](https://www.google.com/webmasters/hacked/) to learn what you can do\.
+ If you believe that the result is a false positive, notify the organization that is reporting the domain\. VirusTotal is an aggregate of several antivirus and URL scanners and cannot remove your domain from a blacklist itself\.

After you correct the problem and the VirusTotal registry has been updated, request a new certificate\.

If you see this error and your domain is not included in the VirusTotal list, visit the [AWS Support Center](https://console.aws.amazon.com/support/home) and create a case\. If you don't have a support agreement, post a message to the [ACM Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.

## Error Message: Additional Verification Required<a name="failed-additional-verification-required"></a>

ACM requires additional information to process this certificate request\. This can happen as a fraud\-protection measure, such as when the domain ranks within the [Alexa top 1000 websites](https://aws.amazon.com/marketplace/pp/Amazon-Web-Services-Alexa-Top-Sites/B07QK2XWNV)\. To provide the required information, use the [Support Center](https://console.aws.amazon.com/support/home) to contact AWS Support\. If you don't have a support plan, post a new thread in the [ACM Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\. 

**Note**  
You cannot request a certificate for Amazon\-owned domain names such as those ending in amazonaws\.com, cloudfront\.net, or elasticbeanstalk\.com\.

## Error Message: Invalid Public Domain<a name="failed-invalid-domain"></a>

One or more of the domain names in the certificate request is not valid\. Typically, this is because a domain name in the request is not a valid top\-level domain\. Try again to request a certificate, correcting any spelling errors or typos that were in the failed request, and ensure that all domain names in the request are for valid top\-level domains\. For example, you cannot request an ACM certificate for example\.invalidpublicdomain because "invalidpublicdomain" is not a valid top\-level domain\. If you continue to receive this failure reason, contact the [Support Center](https://console.aws.amazon.com/support/home)\. If you don't have a support plan, post a new thread in the [ACM Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.

## Error Message: Other<a name="failed-other"></a>

Typically, this failure occurs when there is a typographical error in one or more of the domain names in the certificate request\. Try again to request a certificate, correcting any spelling errors or typos that were in the failed request\. If you continue to receive this failure message, use the [Support Center](https://console.aws.amazon.com/support/home) to contact AWS Support\. If you don't have a support plan, post a new thread in the [ACM Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.