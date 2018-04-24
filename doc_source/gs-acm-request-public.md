# Request a Public Certificate<a name="gs-acm-request-public"></a>

The following sections discuss how to use the ACM console or AWS CLI to request a public ACM certificate\. If you are having trouble requesting a certificate, see [Troubleshoot Certificate Request Problems](troubleshooting-requests.md)\. If you are having trouble requesting a certificate for an \.IO domain, see [Troubleshoot \.IO Domain Problems](troubleshoot-iodomains.md)\. To request a private certificate using your private certificate authority \(CA\), see [Request a Private Certificate](gs-acm-request-private.md)\. 

**Topics**
+ [Requesting a public certificate using the console](#request-public-console)
+ [Requesting a public certificate using the CLI](#request-public-cli)

## Requesting a public certificate using the console<a name="request-public-console"></a>

**To request an ACM public certificate \(console\)**

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. On the **Request a certificate** page, type your domain name\. You can use a fully qualified domain name \(FQDN\) such as **www\.example\.com** or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wildcard in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wildcard name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wildcard certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.

1. To add more domain names to the ACM certificate, choose **Add more names** and type another domain name in the text box that opens\. This is useful for protecting both a bare or apex domain \(like **example\.com**\) and its subdomains \(**\*\.example\.com**\)\. 

1. After you have typed valid domain names, choose **Review and Request** or choose **Cancel** to quit\. 
**Important**  
Unless you choose to opt out, your certificate will be automatically recorded in at least two public certificate transparency databases\. You cannot currently use the console to opt out\. You must use the AWS CLI or the API\. For more information, see [Opting Out of Certificate Transparency Logging](acm-bestpractices.md#best-practices-transparency)\. For general information about transparency logs, see [Certificate Transparency Logging](acm-concepts.md#concept-transparency)\.

1. If the review page correctly contains the information that you provided for your request, choose **Confirm and request**\. The following page shows that your request status is pending validation\.   
![\[Console shows that the certificate request is pending.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-pending-validation-console.png)

   Before ACM issues a certificate, it validates that you own or control the domain names in your certificate request\. You can use either email validation or DNS validation\. If you choose email validation, ACM sends validation email to three contact addresses registered in the WHOIS database and to five common system administration addresses for each domain name\. You or an authorized representative must reply to one of these email messages\. For more information, see [Use Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you use DNS validation, you simply write a CNAME record provided by ACM to your DNS configuration\. For more information about DNS validation, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 
**Note**  
If you are able to edit your DNS configuration, we recommend that you use DNS domain validation rather than email validation\. DNS validation has multiple benefits over email validation\. See [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

## Requesting a public certificate using the CLI<a name="request-public-cli"></a>

Use the [request\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a new public ACM certificate on the command line\. 

```
aws acm request-certificate \
--domain-name www.example.com \
--validation-method DNS \
--idempotency-token 1234 \
--options CertificateTransparencyLoggingPreference=DISABLED
```

This command outputs the Amazon Resource Name \(ARN\) of your new private certificate\.

```
{
    "CertificateArn": "arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012"
}
```