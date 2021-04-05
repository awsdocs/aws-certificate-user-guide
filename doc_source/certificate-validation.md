# Troubleshooting certificate validation<a name="certificate-validation"></a>

If the ACM certificate request status is **Pending validation**, the request is waiting for action from you\. If you chose email validation when you made the request, you or an authorized representative must respond to the validation email messages\. These messages were sent to the registered WHOIS contact addresses and other common email addresses for the requested domain\. For more information, see [Option 2: Email validation](email-validation.md)\. If you chose DNS validation, you must write the CNAME record that ACM created for you to your DNS database\. For more information, see [Option 1: DNS validationDNS validation](dns-validation.md)\. 

**Important**  
You must validate that you own or control every domain name that you included in your certificate request\. If you chose email validation, you will receive validation email messages for each domain\. If you do not, then see [Not receiving validation email](troubleshooting-email-validation.md#troubleshooting-no-mail)\. If you chose DNS validation, you must create one CNAME record for each domain\. 

**Note**  
Public ACM certificates can be installed on Amazon EC2 instances that are connected to a [Nitro Enclave](acm-services.md#acm-nitro-enclave), but not to other Amazon EC2 instances\. For information about setting up a stand\-alone web server on an Amazon EC2 instance not connected to a Nitro Enclave, see [Tutorial: Install a LAMP web server on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html) or [Tutorial: Install a LAMP web server with the Amazon Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html)\.

We recommend that you use DNS validation rather than email validation\.

Consult the following topics if you experience DNS validation problems\.

**Topics**
+ [Troubleshoot DNS validation problems](troubleshooting-DNS-validation.md)
+ [Troubleshoot email validation problems](troubleshooting-email-validation.md)