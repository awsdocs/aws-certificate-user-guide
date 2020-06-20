# Set Up Your Website or Application<a name="setup-website"></a>

You can install your website on an Amazon EC2 Linux or Windows instance\. For more information about Linux Amazon EC2 instances, see [ Amazon Elastic Compute Cloud User Guide for Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)\. For more information about Windows Amazon EC2 instances, see [Amazon Elastic Compute Cloud User Guide for Microsoft Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html)\. 

 Although you install your website on an Amazon EC2 instance, you cannot directly deploy an ACM Certificate on that instance\. You must instead deploy your certificate by using one of the services integrated with ACM\. For more information see [Services Integrated with AWS Certificate Manager](acm-services.md)\.

To get your website up and running quickly on either Windows or Linux, see the following topics\.

**Topics**
+ [Linux Quickstart](#setup-website-linux)
+ [Windows Quickstart](#setup-website-win)

## Linux Quickstart<a name="setup-website-linux"></a>

To create your website or application on a Linux instance, you can choose a Linux Amazon Machine Image \(AMI\) and install an Apache web server on it\. For more information, see [ Tutorial: Installing a LAMP Web Server on Amazon Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-LAMP.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

## Windows Quickstart<a name="setup-website-win"></a>

To acquire a Microsoft Windows server on which you can install your website or application, choose a Windows Server AMI that comes bundled with a Microsoft Internet Information Services \(IIS\) web server\. Then use the default website or create a new one\. You can also install a WIMP server on your Amazon EC2 instance\. For more information, see [Tutorial: Installing a WIMP Server on an Amazon EC2 Instance Running Windows Server](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/install-WIMP.html) in the *Amazon EC2 User Guide for Windows Instances*\. 