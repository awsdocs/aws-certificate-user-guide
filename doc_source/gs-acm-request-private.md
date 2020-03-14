# Request a Private Certificate<a name="gs-acm-request-private"></a>

The following sections discuss how to use the ACM console request a private certificate from an existing private certificate authority \(CA\) that you have previously created using AWS Certificate Manager Private Certificate Authority\. For more information about creating a private CA, see [Create a Private Certificate Authority](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreateCa.html)\. 

Private certificates issued by ACM resemble public certificates issued by ACM\. The certificates have the following restrictions: 
+ You must use DNS subject names\. For more information, see [Domain Names](acm-concepts.md#concept-dn)
+ You can use only a 2048 bit RSA private key algorithm\.
+ The only supported signing algorithm is SHA256WithRSAEncryption\.
+ Each certificate is valid for 13 months\.
+ The private CA must be Active, and the CA private key type must be RSA 2048 or RSA 4096\.
+ ACM renews the certificate automatically, if possible, after 11 months\.

Private certificates issued by ACM PCA do not have the preceding restrictions\. You can use your private CA to create certificates that have any subject name, use any of the supported private key algorithms, any signing algorithm, and any validity period\. This is beneficial if you must identify a subject by a specific name or if you cannot rotate certificates easily\. For more information, see [Issue a Private Certificate](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaIssueCert.html)\. 

**Note**  
The end date of the CA certificate for a private CA must exceed the end date of the requested certificate, or else the certificate request will fail\.

**Topics**
+ [Requesting a Private Certificate Using the ACM Console](#request-private-console)
+ [Requesting a Private Certificate Using the CLI](#request-private-cli)

## Requesting a Private Certificate Using the ACM Console<a name="request-private-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

   Choose **Request a certificate**\.

1. On the **Request a certificate** page, choose **Request a private certificate** and **Request a certificate** to continue\.

1. On the **Select a certificate authority \(CA\)** page, click the **Select a CA** field to view the list of available private CAs\. Choose a CA from the list\. Information about the CA is displayed to help you verify that you have chosen the correct CA\. 
**Note**  
The ACM console displays **Ineligible** for private CAs with ECDSA keys\.

    Choose **Next**\.

1. On the **Add domain names** page, type your domain name\. You can use a fully qualified domain name \(FQDN\), such as **www\.example\.com**, or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wild card in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wild card name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wild card certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.
**Note**  
You do not need to validate the domain of a private certificate\.

1. To add another name, choose **Add another name to this certificate** and type the name in the text box\. This is useful for protecting both a bare or apex domain \(such as **example\.com**\) and its subdomains such as **\*\.example\.com**\)\. 

   When you finish adding names, choose **Next**\.

1. On the **Add tags** page, you can optionally tag your certificate\. Tags are key/value pairs that serve as metadata for identifying and organizing AWS resources\. For a list of ACM tag parameters and for instructions on how to add tags to certificates after creation, see [Tagging AWS Certificate Manager Certificates](tags.md)\. 

   When you finish adding tags, choose **Review and request**\.

1. If the **Review and request** page contains correct information about your request, choose **Confirm and request**\. ACM returns you to the **Certificates** page where you can review information about all of your ACM certificates, both private and public\.

## Requesting a Private Certificate Using the CLI<a name="request-private-cli"></a>

Use the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a private certificate in ACM\. 

```
aws acm request-certificate \
--domain-name www.example.com \
--idempotency-token 12563 \
--options CertificateTransparencyLoggingPreference=DISABLED \
--certificate-authority-arn arn:aws:acm-pca:region:account:\
certificate-authority/12345678-1`234-1234-1234-123456789012
```

This command outputs the Amazon Resource Name \(ARN\) of your new private certificate\.

```
{
    "CertificateArn": "arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012"
}
```