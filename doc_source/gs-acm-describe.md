# Describing ACM certificates<a name="gs-acm-describe"></a>

You can use the ACM console or the AWS CLI to list detailed metadata about your certificates\.

**To view certificate details in the console**

1. Open the ACM console at [https://console\.aws\.amazon\.com/acm/](https://console.aws.amazon.com/acm/) to display your certificates\. You can navigate through multiple pages of certificates using the page numbers at upper\-right\. 

1. To show detailed metadata for a listed certificate, choose the Certificate ID\. A page opens, displaying the following information:
   + **Certificate status**
     + **Identifier** – 32\-byte hexadecimal unique identifier of the certificate
     + **ARN** – An Amazon Resource Name \(ARN\) in the form `arn:aws:acm:region:account:certificate/certificate_ID`
     + **Type** – Identifies the management category of an ACM certificate\. Possible values are: **Amazon Issued** \| **Private** \| **Imported**\. For more information, see [Requesting a public certificate](gs-acm-request-public.md), [Requesting a private PKI certificate](gs-acm-request-private.md), or [Importing certificates into AWS Certificate Manager](import-certificate.md)\.
     + **Status** – The certificate status\. Possible values are: **Expired** \| **Failed** \| **Inactive** \| **Issued** \| **Pending validation**
     + **Detailed status** – Date and time when the certificate was issued or imported
   + **Domains**
     + **Domain** – The fully qualified domain name \(FQDN\) for the certificate\.
     + **Status** – The domain validation status\. Possible values are: **Pending validation** \| **Revoked** \| **Failed** \| **Timed out** \| **Success**
   + **Details**
     + **In use?** – Whether the certificate is associated with an [AWS integrated service](acm-services.md) Possible values are: **Yes** \| **No**
     + **Domain name** – The first fully qualified domain name \(FQDN\) for the certificate\.
     + **Number of additional names** – Number of domain names for which the certificate is valid
     + **Serial number** – 16\-byte hexadecimal serial number of the certificate
     + **Public key info** – The cryptographic algorithm that generated the key pair
     + **Signature algorithm** – The cryptographic algorithm used to sign the certificate\.
     + **Can be used with** – A list of ACM[integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html) that support a certificate with these parameters
     + **Requested at** – Date and time of issuance request
     + **Issued at** – If applicable, the date and time of issuance
     + **Imported at** – If applicable, the date and time of import
     + **Not before** – The start of the validity period of the certificate
     + **Not after** – The expiration date and time of the certificate
     + **Renewal eligibility** – Possible values are: **Eligible** \| **Ineligible**
     + **CA** – The ARN of the signing CA
   + **Tags**
     + **Key**
     + **Value**
   + **Validation state** – If applicable, possible values are: 
     + **Pending** – Validation has been requested and has not completed\.
     + **Timed out** – A requested validation timed out, but you can repeat the request\.
     + **None** – The certificate is for a private PKI or is self\-signed, and does not need validation\. 

**To view certificate details using the AWS CLI**

Use the [describe\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) in the AWS CLI to display certificate details, as shown in the following command:

```
$ aws acm describe-certificate --certificate-arn arn:aws:acm:region:account:certificate/certificate_ID
```

The command returns information similar to the following:

```
{
    "Certificate": {
        "CertificateArn": "arn:aws:acm:region:account:certificate/certificate_ID",
        "Status": "EXPIRED",
        "Options": {
            "CertificateTransparencyLoggingPreference": "ENABLED"
        },
        "SubjectAlternativeNames": [
            "example.com",
            "www.example.com"
        ],
        "DomainName": "gregpe.com",
        "NotBefore": 1450137600.0,
        "RenewalEligibility": "INELIGIBLE",
        "NotAfter": 1484481600.0,
        "KeyAlgorithm": "RSA-2048",
        "InUseBy": [
            "arn:aws:cloudfront::account:distribution/E12KXPQHVLSYVC"
        ],
        "SignatureAlgorithm": "SHA256WITHRSA",
        "CreatedAt": 1450212224.0,
        "IssuedAt": 1450212292.0,
        "KeyUsages": [
            {
                "Name": "DIGITAL_SIGNATURE"
            },
            {
                "Name": "KEY_ENCIPHERMENT"
            }
        ],
        "Serial": "07:71:71:f4:6b:e7:bf:63:87:e6:ad:3c:b2:0f:d0:5b",
        "Issuer": "Amazon",
        "Type": "AMAZON_ISSUED",
        "ExtendedKeyUsages": [
            {
                "OID": "1.3.6.1.5.5.7.3.1",
                "Name": "TLS_WEB_SERVER_AUTHENTICATION"
            },
            {
                "OID": "1.3.6.1.5.5.7.3.2",
                "Name": "TLS_WEB_CLIENT_AUTHENTICATION"
            }
        ],
        "DomainValidationOptions": [
            {
                "ValidationEmails": [
                    "hostmaster@example.com",
                    "admin@example.com",
                    "postmaster@example.com",
                    "webmaster@example.com",
                    "administrator@example.com"
                ],
                "ValidationDomain": "example.com",
                "DomainName": "example.com"
            },
            {
                "ValidationEmails": [
                    "hostmaster@example.com",
                    "admin@example.com",
                    "postmaster@example.com",
                    "webmaster@example.com",
                    "administrator@example.com"
                ],
                "ValidationDomain": "www.example.com",
                "DomainName": "www.example.com"
            }
        ],
        "Subject": "CN=example.com"
    }
}
```