# Certificate and Key Format for Importing<a name="import-certificate-format"></a>

The certificate, certificate chain, and private key \(if any\) are each imported separately and must be PEM–encoded\. PEM stands for Privacy Enhanced Mail\. The PEM format is often used to represent certificates, certificate requests, certificate chains, and keys\. The typical extension for a PEM–formatted file is `.pem`, but it doesn't need to be\. 

The following examples illustrate the format of the files to be imported\. If the components come to you in a single file, use a text editor \(carefully\) to separate them into three files\. Note that if you edit any of the characters in a PEM file incorrectly or if you add one or more spaces to the end of any line, the certificate, certificate chain, or private key will be invalid\. 

**Example 1\. PEM–encoded certificate**  

```
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
```

**Example 2\. PEM–encoded certificate chain**  
A certificate chain contains one or more certificates\. You can use a text editor, the `copy` command in Windows, or the Linux `cat` command to concatenate your certificate files into a chain\. The certificates must be concatenated in order so that each directly certifies the one preceding\. If importing a private certificate, copy the root certificate last\. The following example contains three certificates, but your certificate chain might contain more or fewer\.   
Do not copy your certificate into the certificate chain\.

```
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
```

**Example 3\. PEM–encoded private keys \(private certificate only\)**  
X\.509 version 3 certificates utilize public key algorithms\. When you create an X\.509 certificate or certificate request, you specify the algorithm and the key bit size that must be used to create the private–public key pair\. The public key is placed in the certificate or request\. You must keep the associated private key secret\. Specify the private key when you import the certificate\. The key must be unencrypted\. The following example shows an RSA private key\.   

```
-----BEGIN RSA PRIVATE KEY-----
Base64–encoded private key
-----END RSA PRIVATE KEY-----
```
The next example shows a PEM–encoded elliptic curve private key\. Depending on how you create the key, the parameters block might not be included\. If the parameters block is included, ACM removes it before using the key during the import process\.   

```
-----BEGIN EC PARAMETERS-----
Base64–encoded parameters
-----END EC PARAMETERS-----
-----BEGIN EC PRIVATE KEY-----
Base64–encoded private key
-----END EC PRIVATE KEY-----
```