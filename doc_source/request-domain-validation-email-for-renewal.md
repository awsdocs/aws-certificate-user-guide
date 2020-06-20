# Request a Domain Validation Email for Certificate Renewal<a name="request-domain-validation-email-for-renewal"></a>

After you have configured contact email addresses for your domain \(see [\(Optional\) Configure Email for Your Domain](setup-email.md)\), you can use the AWS Certificate Manager console or the ACM API to request that ACM send you a domain validation email for your certificate renewal\. You should do this in the following circumstances: 
+ You used email validation when initially requesting your ACM certificate\.
+ Your certificate's renewal status is **pending validation**\. For information about determining a certificate's renewal status, see [Check a Certificate's Renewal Status](check-certificate-renewal-status.md)\.
+ You didn't receive or can't find the original domain validation email that ACM sent for certificate renewal\.

**To request that ACM resend the domain validation email \(console\)**

1. Open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Select the check box next to the certificate that requires manual domain validation\. Then choose **Actions**, **Resend validation email**\.

**To request that ACM resend the domain validation email \(ACM API\)**  
Use the [ResendValidationEmail](https://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html) operation in the ACM API\. In doing so, pass the ARN of the certificate, the domain that requires manual validation, and domain where you want to receive the domain validation emails\. The following example shows how to do this with the AWS CLI\. This example contains line breaks to make it easier to read\.

```
$ aws acm resend-validation-email --certificate-arn arn:aws:acm:us-east-2:111122223333:certificate/97b4deb6-8983-4e39-918e-ef1378924e1e
                                  --domain subdomain.example.com
                                  --validation-domain example.com
```