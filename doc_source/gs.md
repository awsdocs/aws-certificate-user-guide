# Getting Started<a name="gs"></a>

Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If the introductory page appears, choose **Get Started**\. Otherwise, choose **Certificate Manager** or **Private CAs** in the left navigation pane\. 

ACM supports SSL/TLS certificates that can be used to enable secure communication across the internet or over an internal network\. You can request a publicly trusted certificate issued by ACM or import a certificate\. Imported certificates can be issued by a third party and publicly trusted, or they can be self\-signed\. You can also use the ACM console to request that a private certificate be issued by a private certificate authority \(CA\) in your organization\. Private certificates are not trusted by default\. Administrators must install them in client trust stores\. 

This documentation primarily discusses public ACM and third party certificates\. It also discusses how to issue a private certificate using an existing private CA\. To learn more about creating and using a private CA, see [AWS Certificate Manager Private Certificate Authority](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\. 

**Topics**
+ [Request a Public Certificate](gs-acm-request-public.md)
+ [Request a Private Certificate](gs-acm-request-private.md)
+ [Export a Private Certificate](gs-acm-export-private.md)
+ [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)
+ [Use Email to Validate Domain Ownership](gs-acm-validate-email.md)
+ [List ACM–Managed Certificates](gs-acm-list.md)
+ [Describe ACM Certificates](gs-acm-describe.md)
+ [Delete ACM–Managed Certificates](gs-acm-delete.md)
+ [Install ACM Certificates](gs-acm-install.md)
+ [Resend Validation Email \(Optional\)](gs-acm-resend.md)