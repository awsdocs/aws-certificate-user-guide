# Exporting a Private Certificate<a name="gs-acm-export-private"></a>

You can export a certificate issued by ACM Private CA for use anywhere in your private PKI environment\. The exported file contains the certificate, the certificate chain, and the encrypted private key\. This file must be stored securely\. For more information about ACM Private CA, see [AWS Certificate Manager Private Certificate Authority User Guide](https://docs.aws.amazon.com/acm-pca/latest/userguide/)\.

**Topics**
+ [Exporting a Private Certificate Using the Console](#export-console)
+ [Export a Private Certificate Using the CLI](#export-cli)

## Exporting a Private Certificate Using the Console<a name="export-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Choose **Certificate Manager**

1. Select the certificate that you want to export\.

1. On the **Actions** menu, choose **Export \(private certificates only\)**\.

1. Enter and confirm a passphrase for the private key\.

1. Choose **Generate PEM Encoding**\.

1. You can copy the certificate, certificate chain, and encrypted key to memory or choose **Export to a file ** for each\.

1. Choose **Done**\.

## Export a Private Certificate Using the CLI<a name="export-cli"></a>

Use the [export\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/export-certificate.html) command to export a private certificate and private key\. You must assign the passphrase when you run the command\. For added security, store your passphrase securely in a file before using the command\. This prevents your passphrase from being stored in the command history and prevents others from seeing the passphrase as you type it in\. 

The following example pipes the command output to `jq` to apply PEM formatting\.

```
aws acm export-certificate \
--certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012 \
--passphrase file://path-to-passphrase-file  \
| jq -r '"\(.Certificate)\(.CertificateChain)\(.PrivateKey)"'
```

This outputs a base64\-encoded, PEM\-format certificate, also containing the certificate chain and encrypted private key, as in the following abbreviated example\.

```
-----BEGIN CERTIFICATE-----
MIIDTDCCAjSgAwIBAgIRANWuFpqA16g3IwStE3vVpTwwDQYJKoZIhvcNAQELBQAw
EzERMA8GA1UECgwIdHJvbG9sb2wwHhcNMTkwNzE5MTYxNTU1WhcNMjAwODE5MTcx
NTU1WjAXMRUwEwYDVQQDDAx3d3cuc3B1ZHMuaW8wggEiMA0GCSqGSIb3DQEBAQUA
...
8UNFQvNoo1VtICL4cwWOdLOkxpwkkKWtcEkQuHE1v5Vn6HpbfFmxkdPEasoDhthH
FFWIf4/+VOlbDLgjU4HgtmV4IJDtqM9rGOZ42eFYmmc3eQO0GmigBBwwXp3j6hoi
74YM+igvtILnbYkPYhY9qz8h7lHUmannS8j6YxmtpPY=
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIC8zCCAdugAwIBAgIRAM/jQ/6h2/MI1NYWX3dDaZswDQYJKoZIhvcNAQELBQAw
EzERMA8GA1UECgwIdHJvbG9sb2wwHhcNMTkwNjE5MTk0NTE2WhcNMjkwNjE5MjA0
NTE2WjATMREwDwYDVQQKDAh0cm9sb2xvbDCCASIwDQYJKoZIhvcNAQEBBQADggEP
...
j2PAOviqIXjwr08Zo/rTy/8m6LAsmm3LVVYKLyPdl+KB6M/+H93Z1/Bs8ERqqga/
6lfM6iw2JHtkW+q4WexvQSoqRXFhCZWbWPZTUpBS0d4/Y5q92S3iJLRa/JQ0d4U1
tWZyqJ2rj2RL+h7CE71XIAM//oHGcDDPaQBFD2DTisB/+ppGeDuB
-----END CERTIFICATE-----
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIFKzBVBgkqhkiG9w0BBQ0wSDAnBgkqhkiG9w0BBQwwGgQUMrZb7kZJ8nTZg7aB
1zmaQh4vwloCAggAMB0GCWCGSAFlAwQBKgQQDViroIHStQgNOjR6nTUnuwSCBNAN
JM4SG202YPUiddWeWmX/RKGg3lIdE+A0WLTPskNCdCAHqdhOSqBwt65qUTZe3gBt
...
ZGipF/DobHDMkpwiaRR5sz6nG4wcki0ryYjAQrdGsR6EVvUUXADkrnrrxuHTWjFl
wEuqyd8X/ApkQsYFX/nhepOEIGWf8Xu0nrjQo77/evhG0sHXborGzgCJwKuimPVy
Fs5kw5mvEoe5DAe3rSKsSUJ1tM4RagJj2WH+BC04SZWNH8kxfOC1E/GSLBCixv3v
+Lwq38CEJRQJLdpta8NcLKnFBwmmVs9OV/VXzNuHYg==
-----END ENCRYPTED PRIVATE KEY-----
```

To output everything to a file, append the `>` redirector to the previous example, yielding the following\. 

```
aws acm export-certificate \
--certificate-arn arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012 \
--passphrase file://path-to-passphrase-file \
| jq -r '"\(.Certificate)\(.CertificateChain)\(.PrivateKey)"' \
> /tmp/export.txt
```