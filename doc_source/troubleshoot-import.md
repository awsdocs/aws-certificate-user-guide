# Troubleshoot Certificate Importing Problems<a name="troubleshoot-import"></a>

Before you can import a certificate into ACM, you must make sure that the certificate, private key, and certificate chain are all PEM\-encoded\. If you edit any of the characters incorrectly or if you if you add one or more spaces to the end of any line, the certificate will be invalid\. You must also ensure that the private key is unencrypted\. Your certificates must be similar to the following examples\. 

**Example PEM\-encoded certificate**  

```
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
```

**Example PEM\-encoded, unencrypted private key**  

```
-----BEGIN RSA PRIVATE KEY-----
Base64-encoded private key
-----END RSA PRIVATEKEY-----
```

**Example PEM\-encoded certificate chain**  
A certificate chain contains one or more certificates\. The following example contains three certificates, but your certificate chain might contain more or fewer\.  

```
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
```