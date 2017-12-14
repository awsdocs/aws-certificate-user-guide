# Request a Certificate<a name="gs-acm-request"></a>

The following sections discuss how to use the ACM console or AWS CLI to request an ACM Certificate\. If you are having trouble requesting a certificate, see [Troubleshoot Certificate Request Problems](troubleshooting-requests.md)\. If you are having trouble requesting a certificate for an \.IO domain, see [Troubleshoot \.IO Domain Problems](troubleshoot-iodomains.md)\.

**To request an ACM Certificate \(console\)**

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If the introductory page appears, choose **Get Started**\. Otherwise, choose **Request a certificate**\. 

1. On the **Request a certificate** page, type your domain name\. You can use a fully qualified domain name \(FQDN\) such as **www\.example\.com** or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wildcard in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wildcard name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM Certificate\. 
**Note**  
When you request a wildcard certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.

1. To add more domain names to the ACM Certificate, choose **Add more names** and type another domain name in the text box that opens\. This is useful for protecting both a bare or apex domain \(like **example\.com**\) and its subdomains \(**\*\.example\.com**\)\.

1. After you have typed valid domain names, choose **Review and Request** or choose **Cancel** to quit\. 

1.  If the review page correctly contains the information that you provided for your request, choose **Confirm and request**\. The following page shows that your request status is pending validation\.   
![\[Console shows that the certificate request is pending.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-pending-validation-console.png)

   Before ACM issues a certificate, it validates that you own or control the domain names in your certificate request\. You can use either email validation or DNS validation\. If you choose email validation, ACM sends validation email to three contact addresses registered in the WHOIS database and to five common system administration addresses for each domain name\. You or an authorized representative must reply to one of these email messages\. For more information, see [Use Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you use DNS validation, you simply write a CNAME record provided by ACM to your DNS configuration\. For more information about DNS validation, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 
**Note**  
If you are able to edit your DNS configuration, we recommend that you use DNS domain validation rather than email validation\. DNS validation has multiple benefits over email validation\. See [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**To request an ACM Certificate \(AWS CLI\)**

+ Use the [request\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a new ACM Certificate on the command line\. 

  ```
  aws acm request-certificate --domain-name www.example.com --validation-method DNS --idempotency-token 1234
  ```

  See the [AWS CLI reference](http://docs.aws.amazon.com/cli/latest/reference/acm/index.html) for more information and examples\.