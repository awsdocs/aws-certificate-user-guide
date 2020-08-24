# Requesting a Public Certificate<a name="gs-acm-request-public"></a>

The following sections discuss how to use the ACM console or AWS CLI to request a public ACM certificate\. 

If you encounter problems when requesting a certificate, see [Troubleshooting Certificate Requests](troubleshooting-cert-requests.md)\. 

To request a certificate for a private PKI using ACM Private CA, see [Requesting a Private Certificate](gs-acm-request-private.md)\. 

**Topics**
+ [Request a Public Certificate Using the Console](#request-public-console)
+ [Request a Public Certificate Using the CLI](#request-public-cli)

## Request a Public Certificate Using the Console<a name="request-public-console"></a>

**To request an ACM public certificate \(console\)**

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

   Choose **Request a certificate**\.

1. On the **Request a certificate** page, choose **Request a public certificate** and **Request a certificate** to continue\.

1. On the **Add domain names** page, type your domain name\. You can use a fully qualified domain name \(FQDN\), such as **www\.example\.com**, or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wild card in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wild card name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wild card certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.

1. To add another name, choose **Add another name to this certificate** and type the name in the text box\. This is useful for protecting both a bare or apex domain \(such as **example\.com**\) and its subdomains such as **\*\.example\.com**\)\. 

   When you finish adding names, choose **Next**\.

1. On the **Select validation method** page, choose either **DNS validation** or **Email validation**, depending on your needs\.
**Note**  
If you are able to edit your DNS configuration, we recommend that you use DNS domain validation rather than email validation\. DNS validation has multiple benefits over email validation\. See [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

   Before ACM issues a certificate, it validates that you own or control the domain names in your certificate request\. You can use either email validation or DNS validation\. If you choose email validation, ACM sends validation email to three contact addresses registered in the WHOIS database and to five common system administration addresses for each domain name\. You or an authorized representative must reply to one of these email messages\. For more information, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you use DNS validation, you simply add a CNAME record provided by ACM to your DNS configuration\. For more information about DNS validation, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\.

   After choosing a validation method, choose **Next**\.

1. On the **Add tags** page, you can optionally tag your certificate\. Tags are key/value pairs that serve as metadata for identifying and organizing AWS resources\. For a list of ACM tag parameters and for instructions on how to add tags to certificates after creation, see [Tagging AWS Certificate Manager Certificates](tags.md)\. 

   When you finish adding tags, choose **Review**\.

1. If the **Review** page contains correct information about your request, choose **Confirm and request**\. A confirmation page shows that your request is being processed and that certificate domains are being validated\. Certificates awaiting validation are in the **Pending validation** state\. 
**Important**  
Unless you choose to opt out, your certificate will be automatically recorded in at least two public certificate transparency databases\. You cannot currently use the console to opt out\. You must use the AWS CLI or the API\. For more information, see [Opting Out of Certificate Transparency Logging](acm-bestpractices.md#best-practices-transparency)\. For general information about transparency logs, see [Certificate Transparency Logging](acm-concepts.md#concept-transparency)\.

   Choose **Continue** to return to the ACM console\.

## Request a Public Certificate Using the CLI<a name="request-public-cli"></a>

Use the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a new public ACM certificate on the command line\. 

```
aws acm request-certificate \
--domain-name www.example.com \
--validation-method DNS \
--idempotency-token 1234 \
--options CertificateTransparencyLoggingPreference=DISABLED
```

This command outputs the Amazon Resource Name \(ARN\) of your new public certificate\.

```
{
    "CertificateArn": "arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012"
}
```