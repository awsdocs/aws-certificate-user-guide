# Request a Domain Validation Email for Certificate Renewal<a name="request-domain-validation-email-for-renewal"></a>

If you initially used email validation, you can use the console, the AWS CLI, or the ACM API to request that ACM resend you a validation email that you can use to assert that you own or control the FQDNs \(fully qualified domain names\) listed in your certificate\. You should do this in the following circumstances:

+ You used email validation when initially requesting your ACM Certificate\.

+ Your certificate's renewal status is **Pending validation**\. For information about determining a certificate's renewal status, see [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)\.

+ You didn't receive or can't find the original domain validation email that ACM sent for certificate renewal\.

**Note**  
 If you have permission to modify the DNS records for your domain, we recommend that you use DNS validation rather than email validation\. For more information, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**To request that ACM resend the domain validation email \(console\)**

1. Open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Select the check box next to the certificate that requires manual domain validation\. Then choose **Actions**, **Resend validation email**\.

**To request that ACM resend the domain validation email \(AWS CLI\)**  
Use the [ResendValidationEmail](http://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html) operation in the ACM API\. Pass the ARN of the certificate, the domain that requires manual validation, and \(optional\) the domain where you want to receive the domain validation emails\. The following example shows how to do this with the AWS CLI\. This example contains line breaks to make it easier to read\.

```
$ aws acm resend-validation-email --certificate-arn arn:aws:acm:region:123456789012 \
  	                   :certificate/97b4deb6-8983-4e39-918e-ef1378924e1e \
                       --domain subdomain.example.com \
                       --validation-domain example.com
```

**To request that ACM resend the domain validation email \(ACM API\)**  
For a Java example that shows how to use the [ResendValidationEmail](http://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html), see [Resending Validation Email](sdk-validate.md)\. 