# Requesting a Private Certificate<a name="gs-acm-request-private"></a>

The following sections discuss how to use the ACM console or an ACM CLI command to request a private certificate signed by a private certificate authority \(CA\) that was previously created using ACM Private CA\. The CA may either reside in your account or be shared with you by a different account\. For more information about creating a private CA, see [Create a Private Certificate Authority](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreateCa.html)\. 

Public and private ACM certificates both follow the X\.509 standard, but certificates intended for public use are subject to the following restrictions: 
+ You must use DNS subject names\. For more information, see [Domain Names](acm-concepts.md#concept-dn)
+ You can use only a 2048 bit RSA private key algorithm\.
+ The only supported signing algorithm is SHA256WithRSAEncryption\.
+ Each certificate is valid for 13 months \(395 days\), and ACM renews the certificate automatically, if possible, after 11 months\.

Certificates signed by a private CA are free of these restrictions\. Instead, you can: 
+ use any subject name
+ use any private\-key algorithm by [supported](https://docs.aws.amazon.com/acm-pca/latest/userguide/supported-algorithms.html) by ACM Private CA
+ use any signing algorithm [supported](https://docs.aws.amazon.com/acm-pca/latest/userguide/supported-algorithms.html) by ACM Private CA
+ specify any validity period

This flexibility is beneficial if you must identify a subject by a specific name or if you cannot rotate certificates easily\.

**Note**  
The end date of the CA certificate for a private CA must exceed the end date of the requested certificate, or else the certificate request will fail\.

The private CA must have a status of Active, and the CA private key type must be RSA 2048 or RSA 4096\.

**Note**  
Because certificates signed by a private CA are not trusted by default, administrators must install them in client trust stores\.

**Topics**
+ [Configuring Access to a Private CA](#ca-access)
+ [Request a Private Certificate Using the ACM Console](#request-private-console)
+ [Request a Private Certificate Using the CLI](#request-private-cli)

## Configuring Access to a Private CA<a name="ca-access"></a>

You can use ACM Private CA to sign your ACM certificates in either of two cases:
+ **Single account**: The signing CA and the ACM certificate that is issued reside in the same AWS account\.

  For single\-account issuance and renewals to be enabled, the ACM Private CA administrator must grant permission to the ACM service principal to create, retrieve, and list certificates\. This is done using the ACM Private CA API action [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html) or the AWS CLI command [create\-permission](https://docs.aws.amazon.com/cli/latest/reference/acm-pca/create-permission.html)\. The account owner assigns these permissions to an IAM user or group responsible for issuing the certificates\.
+ **Cross\-account**: The signing CA and the ACM certificate that is issued reside in different AWS accounts, and access to the CA has been granted to the account where the certificate resides\.

  To enable cross\-account issuance and renewals, the ACM Private CA administrator must attach a resource\-based policy to the CA using the ACM Private CA API action [https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html) or the AWS CLI command [put\-policy](https://docs.aws.amazon.com/cli/latest/reference/acm-pca/create-permission.html)\. The policy white\-lists principals in other accounts that are allowed limited access to the CA\. For more information, see [Using a Resource Based Policy with ACM Private CA](http://docs.aws.amazon.com/acm-pca/latest/userguide/pca-rbp.html)\. 

  The cross\-account scenario also requires ACM to set up a service\-linked role \(SLR\) to interact as a principal with the PCA policy\. Creating an SLR is performed automatically during issuance of the first certificate\. 

  ACM may alert you that it cannot determine whether an SLR exists on your account\. If the required `iam:GetRole` permission has already been granted to the ACM SLR for your account, then the alert will not recur after the SLR is created\. If it does recur, then you or your account administrator may need to grant the `iam:GetRole` permission to ACM, or associate your account with the ACM managed policy `AWSCertificateManagerFullAccess`\.

  For more information, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\. 

## Request a Private Certificate Using the ACM Console<a name="request-private-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

   Choose **Request a certificate**\.

1. On the **Request a certificate** page, choose **Request a private certificate** and **Request a certificate** to continue\.

1. On the **Select a certificate authority \(CA\)** page, click the **Select a CA** field to view the list of available private CAs identified by ARN\. If the CA is shared from another account, the ARN is prefaced by ownership information\. Choose a CA from the list\. 

   Details about the CA are displayed to help you verify that you have chosen the correct CA:
   + **Owner**
   + **Type**
   + **Subject distinguished name**
   + **Value Organization \(O\)**
   + **Organization unit \(OU\)**
   + **Country name \(C\)**
   + **State or province**
   + **Locality name**
   + **Common name \(CN\)**
**Note**  
The ACM console displays **Ineligible** for private CAs with ECDSA keys\.

    Choose **Next**\.

1. On the **Add domain names** page, type your domain name\. You can use a fully qualified domain name \(FQDN\), such as **www\.example\.com**, or a bare or apex domain name such as **example\.com**\. You can also use an asterisk \(**\***\) as a wild card in the leftmost position to protect several site names in the same domain\. For example, **\*\.example\.com** protects **corp\.example\.com**, and **images\.example\.com**\. The wild card name will appear in the **Subject** field and the **Subject Alternative Name** extension of the ACM certificate\. 
**Note**  
When you request a wild card certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com**, and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. To protect both, see the next step\.
**Note**  
You do not need to validate the domain of a private certificate\.

1. To add another name, choose **Add another name to this certificate** and type the name in the text box\. This is useful for authenticating both a bare or apex domain \(such as **example\.com**\) and its subdomains such as **\*\.example\.com**\)\. 

   When you finish adding names, choose **Next**\.

1. On the **Add tags** page, you can optionally tag your certificate\. Tags are key/value pairs that serve as metadata for identifying and organizing AWS resources\. For a list of ACM tag parameters and for instructions on how to add tags to certificates after creation, see [Tagging AWS Certificate Manager Certificates](tags.md)\. 

   When you finish adding tags, choose **Review and request**\.

1. The **Review and request** page displays information about your request\. 

   The first time that you use a CA that is shared with you from another account, ACM alerts you that a service\-linked role \(SLR\) will be created for your account\. This role allows automatic renewal of certificates you sign with the CA\. For more information, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\. 

   If the information is correct, choose **Confirm and request**\. ACM returns you to the **Certificates** page where you can review information about all of your ACM certificates, both private and public\.
**Note**  
ACM may display one of two notices at this point\.   
That ACM cannot determine whether an SLR exists on your account\. This can result from incorrect permission settings\. The certificate request can proceed, but to enable automatic renewal, you or your administrator must supply the needed permission before the certificate expires\. For more information, see [Using a Service Linked Role with ACM](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)\. 
That ACM determined that no SLR exists on your account, and that one will be created for you\.

## Request a Private Certificate Using the CLI<a name="request-private-cli"></a>

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

If the SLR is present, the command out should resemble the following:

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