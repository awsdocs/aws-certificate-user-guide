# Issuing and managing certificates<a name="gs"></a>

ACM certificates can be used to establish secure communications across the internet or within an internal network\. You can request a publicly trusted certificate directly from ACM \(an "ACM certificate"\) or import a publicly trusted certificate issued by a third party\. Self\-signed certificates are also supported\. To provision your organization's internal PKI, you can issue ACM certificates signed by a private certificate authority \(CA\) created and managed by [ACM Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\. The CA may either reside in your account or be shared with you by a different account\. 

**Note**  
Public ACM certificates can be installed on Amazon EC2 instances that are connected to a [Nitro Enclave](acm-services.md#acm-nitro-enclave), but not to other Amazon EC2 instances\. For information about setting up a standalone web server on an Amazon EC2 instance not connected to a Nitro Enclave, see [Tutorial: Install a LAMP web server on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html) or [Tutorial: Install a LAMP web server with the Amazon Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)\.

**Note**  
Because certificates signed by a private CA are not trusted by default, administrators must install them in client trust stores\.

To begin issuing certificates, sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If the introductory page appears, choose **Get Started**\. Otherwise, choose **Certificate Manager** or **Private CAs** in the left navigation pane\. 

**Topics**
+ [Requesting a public certificate](gs-acm-request-public.md)
+ [Requesting a private PKI certificate](gs-acm-request-private.md)
+ [Validating domain ownership](domain-ownership-validation.md)
+ [Listing certificates managed by ACM](gs-acm-list.md)
+ [Describing ACM certificates](gs-acm-describe.md)
+ [Deleting certificates managed by ACM](gs-acm-delete.md)
+ [Installing ACM certificates](gs-acm-install.md)