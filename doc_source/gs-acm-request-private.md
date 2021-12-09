# Requesting a private PKI certificate<a name="gs-acm-request-private"></a>

The following sections discuss how to use the ACM console or an ACM CLI command to request a private PKI certificate that's signed by a private certificate authority \(CA\) that was previously created using ACM Private CA\. The CA may either reside in your account or be shared with you by a different account\. For more information about creating a private CA, see [Create a Private Certificate Authority](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreateCa.html)\. 

**Note**  
Unlike publicly trusted certificates, certificates signed by a private CA do not require validation\.

Public and private ACM certificates both follow the X\.509 standard and are subject to the following restrictions: 
+ You must use DNS subject names\. For more information, see [Domain Names](acm-concepts.md#concept-dn)\.
+ You can use only a 2048\-bit RSA private key algorithm\.
+ The only supported signing algorithm is SHA256WithRSAEncryption\.
+ Each certificate is valid for 13 months \(395 days\)\.
+ ACM renews the certificate automatically, if possible, after 11 months\. 

Certificates signed by a private CA do not have a domain validation step\.

**Note**  
The end date of the CA certificate for a private CA must exceed the end date of the requested certificate, or else the certificate request will fail\.

The private CA must have a status of Active, and the CA private key type must be RSA 2048 or RSA 4096\.

**Note**  
Because certificates signed by a private CA are not trusted by default, administrators must install them in client trust stores\.

**Topics**
+ [Configuring access to a private CA](#ca-access)
+ [Request a private PKI certificate using the ACM console](#request-private-console)
+ [Request a private PKI certificate using the CLI](#request-private-cli)

## Configuring access to a private CA<a name="ca-access"></a>

You can use ACM Private CA to sign your ACM certificates in either of two cases:
+ **Single account**: The signing CA and the ACM certificate that is issued reside in the same AWS account\.

  For single\-account issuance and renewals to be enabled, the ACM Private CA administrator must grant permission to the ACM service principal to create, retrieve, and list certificates\. This is done using the ACM Private CA API action [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html) or the AWS CLI command [create\-permission](https://docs.aws.amazon.com/cli/latest/reference/acm-pca/create-permission.html)\. The account owner assigns these permissions to an IAM user or group that's responsible for issuing the certificates\.
+ **Cross\-account**: The signing CA and the ACM certificate that is issued reside in different AWS accounts, and access to the CA has been granted to the account where the certificate resides\.

  To enable cross\-account issuance and renewals, the ACM Private CA administrator must attach a resource\-based policy to the CA using the ACM Private CA API action [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_PutPolicy.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_PutPolicy.html) or the AWS CLI command [put\-policy](https://docs.aws.amazon.com/cli/latest/reference/acm-pca/put-policy.html)\. The policy specifies principals in other accounts that are allowed limited access to the CA\. For more information, see [Using a Resource Based Policy with ACM Private CA](http://docs.aws.amazon.com/acm-pca/latest/userguide/pca-rbp.html)\. 

  The cross\-account scenario also requires ACM to set up a service\-linked role \(SLR\) to interact as a principal with the PCA policy\. ACM creates the SLR automatically while issuing the first certificate\. 

  ACM might alert you that it cannot determine whether an SLR exists on your account\. If the required `iam:GetRole` permission has already been granted to the ACM SLR for your account, then the alert will not recur after the SLR is created\. If it does recur, then you or your account administrator might need to grant the `iam:GetRole` permission to ACM, or associate your account with the ACM\-managed policy `AWSCertificateManagerFullAccess`\.

  For more information, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\. 

**Important**  
Your ACM certificate must be actively associated with a supported AWS service before it can be automatically renewed\. For information about the resources that ACM supports, see [Services integrated with AWS Certificate Manager](acm-services.md)\. 

## Request a private PKI certificate using the ACM console<a name="request-private-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

   Choose **Request a certificate**\.

1. On the **Request certificate** page, choose **Request a private certificate** and **Next** to continue\.

1. In the **Certificate authority details** section, click the **Certificate authority** menu and choose one of the available private CAs\. If the CA is shared from another account, the ARN is prefaced by ownership information\.

   Details about the CA are displayed to help you verify that you have chosen the correct one:
   + **Owner**
   + **Type**
   + **Common name \(CN\)**
   + **Organization \(O\)**
   + **Organization unit \(OU\)**
   + **Country name \(C\)**
   + **State or province**
   + **Locality name**
   + 
**Note**  
Private CAs with ECDSA keys are not available for issuing certificates and are not displayed in the list of CAs\. 

   

1. In the **Domain names** section, type your domain name\. You can use a fully qualified domain name \(FQDN\), such as **www\.example\.com**, or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wild card in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wild\-card name will appear in the **Subject** field and in the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wild\-card certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.

1. To add another name, choose **Add another name to this certificate** and type the name in the text box\. This is useful for authenticating both a bare or apex domain \(such as **example\.com**\) and its subdomains such as **\*\.example\.com**\)\.

1. In the **Tags** section, you can optionally tag your certificate\. Tags are key\-value pairs that serve as metadata for identifying and organizing AWS resources\. For a list of ACM tag parameters and for instructions on how to add tags to certificates after creation, see [Tagging AWS Certificate Manager certificates](tags.md)\.

1. In the **Certificate renewal permissions** section, acknowledge the notice about certificate renewal permissions\. These permissions allow automatic renewal of private PKI certificates that you sign with the selected CA\. For more information, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\. 

1. After providing all of the required information, choose **Request**\. The console returns you to the certificate list, where you can view your new certificate\.
**Note**  
Depending on how you have ordered the list, a certificate you are looking for might not be immediately visible\. You can click the black triangle at right to change the ordering\. You can also navigate through multiple pages of certificates using the page numbers at upper\-right\.

## Request a private PKI certificate using the CLI<a name="request-private-cli"></a>

Use the [request\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to request a private certificate in ACM\. 

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

In most cases, ACM automatically attaches a service\-linked role \(SLR\) to your account the first time that you use a shared CA\. The SLR enables automatic renewal of end\-entity certificates that you issue\. To check whether the SLR is present, you can query IAM with the following command:

```
aws iam get-role --role-name AWSServiceRoleForCertificateManager
```

If the SLR is present, the command output should resemble the following:

```
{
   "Role":{
      "Path":"/aws-service-role/acm.amazonaws.com/",
      "RoleName":"AWSServiceRoleForCertificateManager",
      "RoleId":"AAAAAAA0000000BBBBBBB",
      "Arn":"arn:aws:iam::{account_no}:role/aws-service-role/acm.amazonaws.com/AWSServiceRoleForCertificateManager",
      "CreateDate":"2020-08-01T23:10:41Z",
      "AssumeRolePolicyDocument":{
         "Version":"2012-10-17",
         "Statement":[
            {
               "Effect":"Allow",
               "Principal":{
                  "Service":"acm.amazonaws.com"
               },
               "Action":"sts:AssumeRole"
            }
         ]
      },
      "Description":"SLR for ACM Service for accessing cross-account Private CA",
      "MaxSessionDuration":3600,
      "RoleLastUsed":{
         "LastUsedDate":"2020-08-01T23:11:04Z",
         "Region":"ap-southeast-1"
      }
   }
}
```

If the SLR is missing, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\.