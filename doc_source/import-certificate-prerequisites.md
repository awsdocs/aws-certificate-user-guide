# Prerequisites for Importing Certificates<a name="import-certificate-prerequisites"></a>

To import an SSL/TLS certificate into ACM, you must provide the certificate and its private key\. If the certificate is not self\-signed, you must also provide a certificate chain\. If the certificate is self\-signed, you can optionally provide a chain\. Also, your certificate must satisfy the following criteria:

+ The certificate must specify an algorithm and key size\. Currently, if you are using the console, you can import a certificate that specifies only the following algorithms: 

  + 1024\-bit RSA \(`RSA_1024`\)

  + 2048\-bit RSA \(`RSA_2048`\)

  If you are using the AWS CLI, you can import a certificate that specifies any of the following:

  + 1024\-bit RSA \(`RSA_1024`\)

  + 2048\-bit RSA \(`RSA_2048`\)

  + 4096\-bit RSA \(`RSA_4096`\)

  + Elliptic Prime Curve 256 bit \(`EC_prime256v1`\)

  + Elliptic Prime Curve 384 bit \(`EC_secp384r1`\)

  + Elliptic Prime Curve 521 bit \(`EC_secp521r1`\)

**Important**  
 Currently, you cannot associate certificates that specify the `RSA_4096`, `EC_prime256v1`, `EC_secp384r1`, or `EC_secp521r1` algorithms and key lengths with any [Services Integrated with AWS Certificate Manager](acm-services.md)\. These are reserved for future use\. Integrated services support only the `RSA_1024` and `RSA_2048` algorithms\. 

+ The certificate must be an SSL/TLS X\.509 version 3 certificate\. You cannot import a certificate for code signing, email encryption, or any other use\. The certificate must contain an RSA public key, the fully qualified domain name \(FQDN\) for your website, and information about the issuing authority\. The certificate can be self\-signed by the private key related to your public key or by the private key of an issuing CA\. You install an SSL/TLS certificate on your web server to enable secure connections between your server and a web browser\.

+ The certificate must be valid at the time of import\. You cannot import a certificate before its validity period begins or after it expires\. The `NotBefore` certificate field contains the validity start date, and the `NotAfter` field contains the end date\.

+ The private key must be unencrypted\. You cannot import a private key that is protected by a password or passphrase\.

+ The certificate, private key, and certificate chain must all be PEM\-encoded\. PEM stands for Privacy Enhanced Mail but was never widely adopted an Internet mail standard\. Instead, the PEM format is often used to represent a certificate or certificate request\. It is base64\-encoded and placed between a `-----BEGIN CERTIFICATE-----` header and an `-----END CERTIFICATE-----` footer\.

  ```
  -----BEGIN CERTIFICATE-----
  Base64-encoded certificate
  -----END CERTIFICATE-----
  ```