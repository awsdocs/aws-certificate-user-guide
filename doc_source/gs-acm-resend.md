# Resending Validation Email \(Optional\)<a name="gs-acm-resend"></a>

You can use email to validate that you own or control a domain\. Each email contains a validation token that you can use to approve a certificate request\. However, because the validation email required for the approval process can be blocked by spam filters or lost in transit, the validation token automatically expires after 72 hours\. If you do not receive the original email or the token has expired, you can request that the email be resent\. 

For persistent problems with email validation, see the [Troubleshoot Email Validation Problems](troubleshooting-email-validation.md) section in [Troubleshooting](troubleshooting.md)\.

**Note**  
The following information applies only to certificates provided by ACM and only to certificates that use email validation\. Validation email is not required for [certificates that you imported into ACM](import-certificate.md)\. For information about DNS domain validation, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**Topics**
+ [Resend Email \(Console\)](#gs-acm-resend-console)
+ [Resend Email \(CLI\)](#gs-acm-resend-cli)

## Resend Email \(Console\)<a name="gs-acm-resend-console"></a>

Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. For a listed certificate showing a status of **Pending validation**, select its check box, choose **Actions**, and then choose **Resend validation email**\. If the 72\-hour period has passed and the certificate status has changed to **Timed out**, you cannot resend validation email\. 

## Resend Email \(CLI\)<a name="gs-acm-resend-cli"></a>

You can use the [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command to resend email\. 

```
aws acm resend-validation-email --certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012 --domain www.example.com --validation-domain example.com
```

**Note**  
The [resend\-validation\-email](https://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command applies only to ACM certificates for which you are using email validation\. Validation is not required for certificates that you have imported into ACM or for private certificates that you manage using ACM\.