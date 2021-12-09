# Requesting a public certificate<a name="gs-acm-request-public"></a>

The following sections discuss how to use the ACM console or AWS CLI to request a public ACM certificate\. 

If you encounter problems when requesting a certificate, see [Troubleshooting certificate requests](troubleshooting-cert-requests.md)\. 

To request a certificate for a private PKI using ACM Private CA, see [Requesting a private PKI certificate](gs-acm-request-private.md)\. 

**Note**  
Unless you choose to opt out, publicly trusted ACM certificates are automatically recorded in at least two certificate transparency databases\. You cannot currently use the console to opt out\. You must use the AWS CLI or the ACM API\. For more information, see [Opting out of certificate transparency logging](acm-bestpractices.md#best-practices-transparency)\. For general information about transparency logs, see [Certificate Transparency Logging](acm-concepts.md#concept-transparency)\.

**Topics**
+ [Request a public certificate using the console](#request-public-console)
+ [Request a public certificate using the CLI](#request-public-cli)

## Request a public certificate using the console<a name="request-public-console"></a>

**To request an ACM public certificate \(console\)**

1. Sign in to the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

   Choose **Request a certificate**\.

1. In the **Domain names** section, type your domain name\. 

   You can use a fully qualified domain name \(FQDN\), such as **www\.example\.com**, or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wild card in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wild\-card name will appear in the **Subject** field and in the **Subject Alternative Name** extension of the ACM certificate\. 

   When you request a wild\-card certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.
**Note**  
In compliance with [RFC 5280](https://datatracker.ietf.org/doc/html/rfc5280), the length of the domain name \(technically, the Common Name\) that you enter in this step cannot exceed 64 octets \(characters\), including periods\. Each subsequent Subject Alternative Name \(SAN\) that you provide, as in the next step, can be up to 253 octets in length\. 

1. To add another name, choose **Add another name to this certificate** and type the name in the text box\. This is useful for protecting both a bare or apex domain \(such as **example\.com**\) and its subdomains such as **\*\.example\.com**\)\.

1. In the **Select validation method** section, choose either **DNS validation** or **Email validation**, depending on your needs\.
**Note**  
If you are able to edit your DNS configuration, we recommend that you use DNS domain validation rather than email validation\. DNS validation has multiple benefits over email validation\. See [DNS validationDNS validation](dns-validation.md)\. 

   Before ACM issues a certificate, it validates that you own or control the domain names in your certificate request\. You can use either email validation or DNS validation\. 

   If you choose email validation, ACM sends validation email to three contact addresses registered in the WHOIS database, and to five common system administration addresses for each domain name\. You or an authorized representative must reply to one of these email messages\. For more information, see [Email validation](email-validation.md)\. 

   If you use DNS validation, you simply add a CNAME record provided by ACM to your DNS configuration\. For more information about DNS validation, see [DNS validationDNS validation](dns-validation.md)\.

1. In the **Tags** page, you can optionally tag your certificate\. Tags are key\-value pairs that serve as metadata for identifying and organizing AWS resources\. For a list of ACM tag parameters and for instructions on how to add tags to certificates after creation, see [Tagging AWS Certificate Manager certificates](tags.md)\. 

   When you finish adding tags, choose **Request**\.

1. After the request is processed, the console returns you to your certificate list, where your new certificate is displayed with status **Pending validation**\.
**Note**  
Depending on how you have ordered the list, a certificate you are looking for might not be immediately visible\. You can click the black triangle at right to change the ordering\. You can also navigate through multiple pages of certificates using the page numbers at upper\-right\.

## Request a public certificate using the CLI<a name="request-public-cli"></a>

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