# Request a Private Certificate<a name="gs-acm-request-private"></a>

The following sections discuss how to use the ACM console or the ACM PCA CLI request a private certificate from an existing private certificate authority \(CA\)\. For more information about creating a private CA, see [Create a Private Certificate Authority](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreateCa.html)\. 

Private certificates issued by ACM resemble public certificates issued by ACM\. The certificates have the following restrictions: 
+ You must use DNS subject names\. For more information, see [Domain Names](acm-concepts.md#concept-dn)
+ You can use only a 2048 bit RSA private key algorithm\.
+ The only supported signing algorithm is SHA256WithRSAEncryption\.
+ Each certificate is valid for 13 months\.
+ The private CA must be Active, and the CA private key type must be RSA 2048 or RSA 4096\.
+ ACM renews the certificate automatically, if possible, after 11 months\.

Private certificates issued by ACM PCA do not have the preceding restrictions\. You can use your private CA to create certificates that have any subject name, use any of the supported private key algorithms, any signing algorithm, and any validity period\. This is beneficial if you must identify a subject by a specific name or if you cannot rotate certificates easily\. For more information, see [Issue a Private Certificate](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaIssueCert.html)\. 

**Topics**
+ [Requesting a private certificate using the console](#request-private-console)
+ [Requesting a private certificate using the CLI](#request-private-cli)

## Requesting a private certificate using the console<a name="request-private-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Select **Request a private certificate** and then choose **Request a certificate**\.

1. Select your private CA from the dropdown list\. Information about the CA is filled in below the list to help you verify that you have chosen the CA you want\. 
**Note**  
The ACM console displays **Ineligible** for private CAs with ECDSA keys\.

1.  Choose **Next**\.

1. On the **Request a certificate** page, type a domain name\. You can use a fully qualified domain name \(FQDN\) such as **www\.example\.com** or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wildcard in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wildcard name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wildcard certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.

1. To add more domain names to the ACM certificate, choose **Add more names** and type another domain name in the text box that opens\. This is useful for protecting both a bare or apex domain \(like **example\.com**\) and its subdomains \(**\*\.example\.com**\)\. 

1. After you have typed valid names, choose **Review and Request** or choose **Cancel** to quit\. 

1. Check the review page to make sure that everything is correct and then choose **Confirm and request**\. 
**Note**  
You do not need to validate a private certificate\.

## Requesting a private certificate using the CLI<a name="request-private-cli"></a>

Use the [request\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a private certificate in ACM\. 

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