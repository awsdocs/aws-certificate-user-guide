# AWS CloudHSM Use Cases<a name="use-cases"></a>

A hardware security module \(HSM\) in AWS CloudHSM can help you accomplish a variety of goals\.


+ [Offload the SSL/TLS Processing for Web Servers](#crypto-offload)
+ [Protect the Private Keys for an Issuing Certificate Authority \(CA\)](#certificate-authority)
+ [Enable Transparent Data Encryption \(TDE\) for Oracle Databases](#transparent-data-encryption)

## Offload the SSL/TLS Processing for Web Servers<a name="crypto-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive webpages or other data over the internet\. This is commonly known as HTTPS\. The web server uses a public–private key pair and a public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this computation to the HSMs in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. This offloading reduces the computational burden on your web server and provides extra security by storing the server's private key in the HSMs\.

For information about setting up SSL/TLS offload with AWS CloudHSM, see [SSL/TLS Offload](ssl-offload.md)\.

## Protect the Private Keys for an Issuing Certificate Authority \(CA\)<a name="certificate-authority"></a>

In a public key infrastructure \(PKI\), a certificate authority \(CA\) is a trusted entity that issues digital certificates\. These digital certificates bind a public key to an identity \(a person or organization\) by means of public key cryptography and digital signatures\. To operate a CA, you must maintain trust by protecting the private keys that sign the certificates issued by your CA\. You can store these private keys in an HSM and use the HSM to perform the cryptographic signing operations\.

## Enable Transparent Data Encryption \(TDE\) for Oracle Databases<a name="transparent-data-encryption"></a>

Some versions of Oracle's database software offer a feature called Transparent Data Encryption \(TDE\)\. With TDE, the database software encrypts data before storing it on disk\. The data in the database's table columns or tablespaces is encrypted with a table key or tablespace key\. These keys are encrypted with the TDE master encryption key\. You can store the TDE master encryption key in the HSMs in your AWS CloudHSM cluster, which provides additional security\.

For information about setting up Oracle TDE with AWS CloudHSM, see [Oracle Database Encryption](oracle-tde.md)\.