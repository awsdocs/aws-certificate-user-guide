# Export a Private Certificate<a name="gs-acm-export-private"></a>

You can export a private certificate for use anywhere\. You can export the certificate, the certificate chain, and the encrypted private key\. You must store the private key securely\. The key is related to the public key that is embedded in the certificate\. 

The private key is a 2048 bit RSA key\. You can use the following OpenSSL command to decrypt it\. Provide the passphrase when prompted\. 

```
openssl rsa -in encrypted_key.pem -out decrypted_key.pem
```

**Topics**
+ [Exporting a private certificate using the console](#export-console)
+ [Exporting a private certificate using the CLI](#export-cli)

## Exporting a private certificate using the console<a name="export-console"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\.

1. Choose **Certificate Manager**

1. Select the certificate that you want to export\.

1. On the **Actions** menu, choose **Export \(private certificates only\)**\.

1. Enter and confirm a passphrase for the private key\.

1. Choose **Generate PEM Encoding**\.

1. You can copy the certificate, certificate chain, and encrypted key to memory or choose **Export to a file ** for each\.

1. Choose **Done**\.

## Exporting a private certificate using the CLI<a name="export-cli"></a>

Use the [export\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/export-certificate.html) command to export a private certificate and private key\. For added security, store your passphrase securely in a file before using this command\. This prevents your passphrase from being stored in command history and prevents others from seeing the passphrase as you type it in\. 

```
aws acm export-certificate --certificate-arn \
arn:aws:acm:region:account:\
certificate/12345678-1234-1234-1234-123456789012 \
--passphrase --file://path-to-passphrase-file
```

This command outputs the base64\-encoded, PEM format certificate, the certificate chain, and private key\. The private key is output in PKCS \#8 syntax\. 

```
{
    "PrivateKey": 
      "-----BEGIN ENCRYPTED PRIVATE KEY----- 
        ...PKCS8 Base64-encoded encrypted private key ...
       -----END ENCRYPTED PRIVATE KEY-----",
    "CertificateChain": 
       "-----BEGIN CERTIFICATE-----   
        ...Base64-encoded certificate...
        -----END CERTIFICATE-----
        -----BEGIN CERTIFICATE-----
        ...Base64-encoded private key...
        -----END CERTIFICATE-----",
    "Certificate": 
      "-----BEGIN CERTIFICATE----- 
        ...Base64-encoded certificate...
       -----END CERTIFICATE-----"
}
```

To output everything to a file, use the `>` redirector as shown in the following example\. 

```
aws acm export-certificate --certificate-arn \
arn:aws:acm:region:account:\
certificate/12345678-1234-1234-1234-123456789012 \
--passphrase file://path-to-passphrase-file\
> c:\temp\export.txt
```