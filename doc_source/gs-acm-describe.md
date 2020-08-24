# Describing ACM Certificates<a name="gs-acm-describe"></a>

You can use the ACM console or the AWS CLI to list metadata about your certificates\.

**Topics**
+ [Describe Certificates \(Console\)](#gs-acm-describe-console)
+ [Describe Certificates \(CLI\)](#gs-acm-describe-cli)

## Describe Certificates \(Console\)<a name="gs-acm-describe-console"></a>

To show certificate metadata, select the arrow to the immediate left of the domain name\. The console displays information similar to the following\. 

![\[Certificate columns.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-metadata-console.png)

## Describe Certificates \(CLI\)<a name="gs-acm-describe-cli"></a>

You can use the AWS CLI to get information about an issued certificate, delete a certificate, or resend validation email\.

### Retrieve ACM Certificate Fields<a name="gs-acm-manage-cli-describe"></a>

You can use the [describe\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) command list the metadata for a certificate\. 

```
aws acm describe-certificate --certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012
```

The `describe-certificate` command outputs the following information\.

```
{
    "Certificate": {
        "CertificateArn": "arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012",
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