# Prerequisites for importing certificates<a name="import-certificate-prerequisites"></a>

To import a self–signed SSL/TLS certificate into ACM, you must provide both the certificate and its private key\. To import a certificate signed by a non\-AWScertificate authority \(CA\), you must also include the certificate chain, but not a private key\. Your certificate must satisfy the following criteria listed below\.

For all imported certificates, you must specify a cryptographic algorithm and a key size\. ACM supports the following algorithms \(API name in parentheses\): 
+ 1024\-bit RSA \(`RSA_1024`\)
+ 2048\-bit RSA \(`RSA_2048`\)
+ 3072\-bit RSA \(`RSA_3072`\)
+ 4096\-bit RSA \(`RSA_4096`\)
+ Elliptic Prime Curve 256 bit \(`EC_prime256v1`\)
+ Elliptic Prime Curve 384 bit \(`EC_secp384r1`\)
+ Elliptic Prime Curve 521 bit \(`EC_secp521r1`\)

Note also the following additional requirements: 
+ ACM [integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html) allow only the algorithms and key sizes that they support to be associated with their resources\. For example, CloudFront supports 1024\-bit RSA, 2048\-bit RSA, and Elliptic Prime Curve 256\-bit keys only, while Application Load Balancer supports all of the algorithms available from ACM\. For more information, see the documentation for the service you are using\.
+ A certificate must be an SSL/TLS X\.509 version 3 certificate\. It must contain a public key, the fully qualified domain name \(FQDN\) or IP address for your website, and information about the issuer\. 
+ A certificate can be self\-signed by a private key that you own or by the private key of an issuing CA\. 

  If the certificate is self\-signed, you must provide the private key\. The private key must be no larger than 5 KB \(5,120 bytes\), and it must be unencrypted\.

  If the certificate is signed by a CA, you must provide the certificate chain, and the cryptographic algorithm of the certificate must match the algorithm of the CA\. For example, if the CA key type is RSA, then the certificate key type must also be RSA\.
+ A certificate must be valid at the time of import\. You cannot import a certificate before its validity period begins or after it expires\. The `NotBefore` certificate field contains the validity start date, and the `NotAfter` field contains the end date\.
+ All of the required certificate materials \(certificate, private key, and certificate chain\) must be PEM–encoded\. Uploading DER–encoded materials results in an error\. For more information and examples, see [Certificate and key format for importing](import-certificate-format.md)\.
+ When you renew \(reimport\) a certificate, you cannot add a `KeyUsage` or `ExtendedKeyUsage` extension if the extension was not present in the previously imported certificate\.