# Issuing and Managing Certificates<a name="gs"></a>

ACM certificates can be used to establish secure communications across the internet or within an internal network\. You can request a publicly trusted certificate directly from ACM \(an "ACM certificate"\) or import a publicly trusted certificate issued by a third party\. Self\-signed certificates are also supported\. To provision you organization's internal PKI, you can issue ACM certificates signed by a private certificate authority \(CA\) created and managed by [ACM Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\. The CA may either reside in your account or be shared with you by a different account\. 

**Note**  
Because certificates signed by a private CA are not trusted by default, administrators must install them in client trust stores\.

To begin issuing certificates, sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If the introductory page appears, choose **Get Started**\. Otherwise, choose **Certificate Manager** or **Private CAs** in the left navigation pane\. 

**Topics**
+ [Requesting a Public Certificate](gs-acm-request-public.md)
+ [Requesting a Private Certificate](gs-acm-request-private.md)
+ [Exporting a Private Certificate](gs-acm-export-private.md)
+ [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)
+ [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)
+ [Listing Certificates Managed by ACM](gs-acm-list.md)
+ [Describing ACM Certificates](gs-acm-describe.md)
+ [Deleting Certificates Managed by ACM](gs-acm-delete.md)
+ [Installing ACM Certificates](gs-acm-install.md)
+ [Resending Validation Email \(Optional\)](gs-acm-resend.md)