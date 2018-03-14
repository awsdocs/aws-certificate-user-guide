# AWS CloudHSM Software Library for OpenSSL<a name="openssl-library"></a>

The AWS CloudHSM software library for OpenSSL is an OpenSSL dynamic engine that supports the OpenSSL command line interface and EVP APIs\. The software library allows applications that are integrated with OpenSSL, such as the Nginx and Apache web servers, to offload their cryptographic processing to the HSMs in your AWS CloudHSM cluster\. The engine supports the following key types and ciphers:

+ RSA key generation for 2048, 3072, and 4096\-bit keys\.

+ RSA sign/verify\.

+ RSA encrypt/decrypt\.

+ Random number generation that is cryptographically secure and FIPS\-validated\.

For more information, see the following topic\.


+ [Install and Use the AWS CloudHSM Software Library for OpenSSL](openssl-library-install.md)