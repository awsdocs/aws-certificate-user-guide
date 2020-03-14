# Import a Certificate<a name="import-certificate-api-cli"></a>

You can import a certificate into ACM by using the AWS Management Console, the AWS CLI, or the ACM API\. The following topics show you how to use the AWS Management Console and the AWS CLI\. 

**Topics**
+ [Import Using the Console](#import-certificate-api)
+ [Import Using the AWS CLI](#import-certificate-cli)

## Import Using the Console<a name="import-certificate-api"></a>

The following example shows how to import a certificate using the AWS Management Console\.

1. Open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If this is your first time using ACM, look for the **AWS Certificate Manager** heading and choose the **Get started** button under it\.

1. Choose **Import a certificate**\.

1. Do the following:

   1. For **Certificate body**, paste the PEM\-encoded certificate to import\.

   1.  For **Certificate private key**, paste the PEM\-encoded, unencrypted private key that matches the certificate's public key\. 
**Important**  
 Currently, [Services Integrated with AWS Certificate Manager](acm-services.md) support only the `RSA_1024` and `RSA_2048` algorithms\. 

   1. \(Optional\) For **Certificate chain**, paste the PEM\-encoded certificate chain\.

1. Choose **Review and import**\.

1. Review the information about your certificate, then choose **Import**\.

## Import Using the AWS CLI<a name="import-certificate-cli"></a>

The following example shows how to import a certificate using the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/)\. The example assumes the following:
+ The PEM\-encoded certificate is stored in a file named `Certificate.pem`\.
+ The PEM\-encoded certificate chain is stored in a file named `CertificateChain.pem`\.
+ The PEM\-encoded, unencrypted private key is stored in a file named `PrivateKey.pem`\.

To use the following example, replace the file names with your own and type the command on one continuous line\. The following example includes line breaks and extra spaces to make it easier to read\.

```
  	$ aws acm import-certificate --certificate file://Certificate.pem
                                 --certificate-chain file://CertificateChain.pem
                                 --private-key file://PrivateKey.pem
```

If the `import-certificate` command is successful, it returns the [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) of the imported certificate\. 