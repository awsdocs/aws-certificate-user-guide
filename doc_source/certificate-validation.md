# Troubleshooting Certificate Validation<a name="certificate-validation"></a>

If the ACM certificate request status is **Pending validation**, the request is waiting for action from you\. If you chose email validation when you made the request, you or an authorized representative must respond to the validation email messages\. These messages were sent to the registered WHOIS contact addresses and other common email addresses for the requested domain\. For more information, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md)\. If you chose DNS validation, you must write the CNAME record that ACM created for you to your DNS database\. For more information, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**Important**  
You must validate that you own or control every domain name that you included in your certificate request\. If you chose email validation, you will receive validation email messages for each domain\. If you do not, then see [Not Receiving Validation Email](troubleshooting-email-validation.md#troubleshooting-no-mail)\. If you chose DNS validation, you must create one CNAME record for each domain\. 

**Note**  
Public ACM certificates cannot be installed on Amazon EC2 instances\. For information about setting up a stand\-alone EC2\-based web server, see [Tutorial: Install a LAMP web server on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html) or [Tutorial: Install a LAMP web server with the Amazon Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)\.

We recommend that you use DNS validation rather than email validation\.

Consult the following topics if you experience DNS validation problems\.

**Topics**
+ [Troubleshoot DNS Validation Problems](troubleshooting-DNS-validation.md)
+ [Troubleshoot Email Validation Problems](troubleshooting-email-validation.md)