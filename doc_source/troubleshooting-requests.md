# Troubleshoot Certificate Request Problems<a name="troubleshooting-requests"></a>

Consult the following topics if you have trouble requesting an ACM Certificate\.


+ [Certificate Request Timed Out](#troubleshooting-timed-out)
+ [Certificate Request Failed](#troubleshooting-failed)

## Certificate Request Timed Out<a name="troubleshooting-timed-out"></a>

Requests for ACM Certificates time out if they are not validated within 72 hours\. To correct this condition, delete your request and choose **Request a certificate** to begin again\. You can use DNS validation or email validation to assert that you own or control the domains listed in your request\. We recommend that you use DNS\. For more information see, [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\.

## Certificate Request Failed<a name="troubleshooting-failed"></a>

A request for an ACM Certificate can fail\. If that happens, the following explanations can help you understand why the request failed and suggest steps you can take to fix the problem\. 


+ [No Available Contacts](#failed-no-available-contacts)
+ [Domain Not Allowed](#failed-domain-not-allowed)
+ [Additional Verification Required](#failed-additional-verification-required)
+ [Invalid Public Domain](#failed-invalid-domain)
+ [Other](#failed-other)

### No Available Contacts<a name="failed-no-available-contacts"></a>

You chose email validation when requesting a certificate, but ACM could not find an email address to use for validating one or more of the domain names in the request\. To correct this problem, you can do one of the following:

+ Ensure that you have a working email address that is registered in WHOIS and that the address is visible when performing a standard WHOIS lookup for the domain names in the certificate request\. Typically, you do this through your domain registrar\.

+ Ensure your domain is configured to receive email\. Your domain's name server must have a mail exchanger record \(MX record\) so ACM's email servers know where to send the domain validation email\.

Accomplishing one of the preceding tasks is enough to correct this problem; you don't need to do both\. After you correct the problem, request a new certificate\. You cannot resubmit a failed certificate request\.

For more information about how to ensure that you receive domain validation emails from ACM, see [\(Optional\) Configure Email for Your Domain](setup-email.md) or [Not Receiving Validation Email](troubleshooting-email.md#troubleshooting-no-mail)\. If you follow these steps and continue to get the **No Available Contacts** message, then [report this to AWS](https://console.aws.amazon.com/support/home) so that we can investigate it\.

### Domain Not Allowed<a name="failed-domain-not-allowed"></a>

ACM did not allow you to request a certificate for one or more of the domain names you specified\. Typically, this is because one or more of the domain names in the certificate request was found in the Google Safe Browsing list of unsafe websites or the PhishTank list of valid phishes\. To correct this problem, you can do the following: 

+ Search for your domain name at the [Google Safe Browsing Site Status](https://www.google.com/transparencyreport/safebrowsing/diagnostic/) website\. If your domain is considered unsafe, see [Google Help for Hacked Websites](https://developers.google.com/webmasters/hacked/) to learn what you can do\. If you think your domain is safe, see [Request a review](https://developers.google.com/webmasters/hacked/docs/request_review) to request a review from Google\.

+ Search for your domain name on the [PhishTank home page](https://www.phishtank.com/index.php)\. If your domain is considered a phish, see [Google Help for Hacked Websites](https://developers.google.com/webmasters/hacked/) or [StopBadware Webmaster Help](https://www.stopbadware.org/webmaster-help) to learn what you can do\. If you think your domain is safe, see the [PhishTank FAQ](https://www.phishtank.com/faq.php) for information about how to report a false positive\.

After you correct the problem, request a new certificate\. You cannot resubmit a failed certificate request\.

### Additional Verification Required<a name="failed-additional-verification-required"></a>

ACM requires additional information to process this certificate request\. To provide this information, use the [Support Center](https://console.aws.amazon.com/support/home) to contact AWS Support\. If you don't have a support plan, post a new thread in the [AWS Certificate Manager discussion forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\. 

**Note**  
You cannot request a certificate for Amazon\-owned domain names such as those ending in amazonaws\.com, cloudfront\.net, or elasticbeanstalk\.com\.

### Invalid Public Domain<a name="failed-invalid-domain"></a>

One or more of the domain names in the certificate request is not valid\. Typically, this is because a domain name in the request is not a valid top\-level domain\. Try to request a certificate again, correcting any spelling errors or typos that were in the failed request, and ensuring that all domain names in the request are for valid top\-level domains\. For example, you cannot request an ACM Certificate for example\.invalidpublicdomain because "invalidpublicdomain" is not a valid top\-level domain\. If you continue to receive this failure reason, contact the [Support Center](https://console.aws.amazon.com/support/home)\. If you don't have a support plan, post a new thread in the [AWS Certificate Manager discussion forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.

### Other<a name="failed-other"></a>

Typically, this failure occurs when there is a typographical error in one or more of the domain names in the certificate request\. Try to request a certificate again, correcting any spelling errors or typos that were in the failed request\. If you continue to receive this failure reason, use the [Support Center](https://console.aws.amazon.com/support/home) to contact AWS Support\. If you don't have a support plan, post a new thread in the [AWS Certificate Manager discussion forum](https://forums.aws.amazon.com/forum.jspa?forumID=206)\.