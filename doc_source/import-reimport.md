# Reimport a Certificate<a name="import-reimport"></a>

If you imported a certificate and associated it with other AWS services, you can reimport that certificate before it expires while preserving the AWS service associations of the original certificate\. For more information about AWS services integrated with ACM, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

 The following conditions apply when you reimport a certificate: 
+ You can add or remove domain names\.
+ You cannot remove all of the domain names from a certificate\.
+ You can add new **Key Usage** extensions but existing extension values cannot be removed\.
+ You can add new **Extended Key Usage** extensions but existing extension values cannot be removed\.
+ The key type and size cannot be changed\.
+ You cannot apply resource tags when reimporting a certificate\.

**Topics**
+ [Reimporting Using the Console](#reimport-certificate-api)
+ [Reimporting Using the AWS CLI](#reimport-certificate-cli)

## Reimporting Using the Console<a name="reimport-certificate-api"></a>

The following example shows how to reimport a certificate using the AWS Management Console\.

1. Open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Select or expand the certificate to reimport\.

1. Open the details pane of the certificate and choose the **Reimport certificate** button\. If you selected the certificate by checking the box beside its name, choose **Reimport certificate** on the **Actions** menu\.

1. For **Certificate body**, paste the PEM\-encoded end\-entity certificate\.

1. For **Certificate private key**, paste the unencrypted PEM\-encoded private key associated with the certificate's public key\.
**Important**  
 Currently, [Services Integrated with AWS Certificate Manager](acm-services.md) support only the `RSA_1024` and `RSA_2048` algorithms\. 

1. \(Optional\) For **Certificate chain**, paste the PEM\-encoded certificate chain\. The certificate chain includes one or more certificates for all intermediate issuing certification authorities, and the root certificate\. If the certificate to be imported is self\-assigned, no certificate chain is necessary\.

1. Choose **Review and import**\.

1. Review the information about your certificate\. If there are no errors, choose **Reimport**\.

## Reimporting Using the AWS CLI<a name="reimport-certificate-cli"></a>

The following example shows how to reimport a certificate using the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/)\. The example assumes the following:
+ The PEM\-encoded certificate is stored in a file named `Certificate.pem`\.
+ The PEM\-encoded certificate chain is stored in a file named `CertificateChain.pem`\.
+ \(Private certificates only\) The PEM\-encoded, unencrypted private key is stored in a file named `PrivateKey.pem`\.
+ You have the ARN of the certificate you want to reimport\.

To use the following example, replace the file names and the ARN with your own and type the command on one continuous line\. The following example includes line breaks and extra spaces to make it easier to read\.

**Note**  
To reimport a certificate, you must specify the certificate ARN\.

```
  	$ aws acm import-certificate --certificate file://Certificate.pem
                                 --certificate-chain file://CertificateChain.pem
                                 --private-key file://PrivateKey.pem
                                 --certificate-arn arn:aws:acm:region:123456789012:certificate/12345678-1234-1234-1234-12345678901
```

If the `import-certificate` command is successful, it returns the [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) of the certificate\. 