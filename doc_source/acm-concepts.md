# Concepts<a name="acm-concepts"></a>

This section provides definitions of concepts used by AWS Certificate Manager\.

**Topics**
+ [ACM Certificate](#concept-acm-cert)
+ [ACM Root CAs](#ACM-root-CAs)
+ [Apex Domain](#concept-apex)
+ [Asymmetric Key Cryptography](#concept-asymmetric)
+ [Certificate Authority](#concept-ca)
+ [Certificate Transparency Logging](#concept-transparency)
+ [Domain Name System](#concept-dns)
+ [Domain Names](#concept-dn)
+ [Encryption and Decryption](#concept-encrypt)
+ [Fully Qualified Domain Name \(FQDN\)](#concept-fqdn)
+ [Public Key Infrastructure](#concept-pki)
+ [Root Certificate](#concept-root)
+ [Secure Sockets Layer \(SSL\)](#concept-ssl)
+ [Secure HTTPS](#concept-https)
+ [SSL Server Certificates](#concept-sslcert)
+ [Symmetric Key Cryptography](#concept-symmetric)
+ [Transport Layer Security \(TLS\)](#concept-tls)
+ [Trust](#concept-trust)

## ACM Certificate<a name="concept-acm-cert"></a>

ACM generates X\.509 version 3 certificates\. Each is valid for 13 months \(395 days\) and contains the following extensions\. 
+ **Basic Constraints**\- specifies whether the subject of the certificate is a certification authority \(CA\)
+ **Authority Key Identifier**\- enables identification of the public key corresponding to the private key used to sign the certificate\. 
+ **Subject Key Identifier**\- enables identification of certificates that contain a particular public key\. 
+ **Key Usage**\- defines the purpose of the public key embedded in the certificate\. 
+ **Extended Key Usage**\- specifies one or more purposes for which the public key may be used in addition to the purposes specified by the **Key Usage** extension\. 
+ **CRL Distribution Points**\- specifies where CRL information can be obtained\. 

The plaintext of an ACM\-issued certificate resembles the following example:

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            f2:16:ad:85:d8:42:d1:8a:3f:33:fa:cc:c8:50:a8:9e
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: O=Example CA
        Validity
            Not Before: Jan 30 18:46:53 2018 GMT
            Not After : Jan 31 19:46:53 2018 GMT
        Subject: C=US, ST=VA, L=Herndon, O=Amazon, OU=AWS, CN=example.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:ba:a6:8a:aa:91:0b:63:e8:08:de:ca:e7:59:a4:
                    69:4c:e9:ea:26:04:d5:31:54:f5:ec:cb:4e:af:27:
                    e3:94:0f:a6:85:41:6b:8e:a3:c1:c8:c0:3f:1c:ac:
                    a2:ca:0a:b2:dd:7f:c0:57:53:0b:9f:b4:70:78:d5:
                    43:20:ef:2c:07:5a:e4:1f:d1:25:24:4a:81:ab:d5:
                    08:26:73:f8:a6:d7:22:c2:4f:4f:86:72:0e:11:95:
                    03:96:6d:d5:3f:ff:18:a6:0b:36:c5:4f:78:bc:51:
                    b5:b6:36:86:7c:36:65:6f:2e:82:73:1f:c7:95:85:
                    a4:77:96:3f:c0:96:e2:02:94:64:f0:3a:df:e0:76:
                    05:c4:56:a2:44:72:6f:8a:8a:a1:f3:ee:34:47:14:
                    bc:32:f7:50:6a:e9:42:f5:f4:1c:9a:7a:74:1d:e5:
                    68:09:75:19:4b:ac:c6:33:90:97:8c:0d:d1:eb:8a:
                    02:f3:3e:01:83:8d:16:f6:40:39:21:be:1a:72:d8:
                    5a:15:68:75:42:3e:f0:0d:54:16:ed:9a:8f:94:ec:
                    59:25:e0:37:8e:af:6a:6d:99:0a:8d:7d:78:0f:ea:
                    40:6d:3a:55:36:8e:60:5b:d6:0d:b4:06:a3:ac:ab:
                    e2:bf:c9:b7:fe:22:9e:2a:f6:f3:42:bb:94:3e:b7:
                    08:73
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Authority Key Identifier:
                keyid:84:8C:AC:03:A2:38:D9:B6:81:7C:DF:F1:95:C3:28:31:D5:F7:88:42
            X509v3 Subject Key Identifier:
                97:06:15:F1:EA:EC:07:83:4C:19:A9:2F:AF:BA:BB:FC:B2:3B:55:D8
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://example.com/crl

    Signature Algorithm: sha256WithRSAEncryption
         69:03:15:0c:fb:a9:39:a3:30:63:b2:d4:fb:cc:8f:48:a3:46:
         69:60:a7:33:4a:f4:74:88:c6:b6:b6:b8:ab:32:c2:a0:98:c6:
         8d:f0:8f:b5:df:78:a1:5b:02:18:72:65:bb:53:af:2f:3a:43:
         76:3c:9d:d4:35:a2:e2:1f:29:11:67:80:29:b9:fe:c9:42:52:
         cb:6d:cd:d0:e2:2f:16:26:19:cd:f7:26:c5:dc:81:40:3b:e3:
         d1:b0:7e:ba:80:99:9a:5f:dd:92:b0:bb:0c:32:dd:68:69:08:
         e9:3c:41:2f:15:a7:53:78:4d:33:45:17:3e:f2:f1:45:6b:e7:
         17:d4:80:41:15:75:ed:c3:d4:b5:e3:48:8d:b5:0d:86:d4:7d:
         94:27:62:84:d8:98:6f:90:1e:9c:e0:0b:fa:94:cc:9c:ee:3a:
         8a:6e:6a:9d:ad:b8:76:7b:9a:5f:d1:a5:4f:d0:b7:07:f8:1c:
         03:e5:3a:90:8c:bc:76:c9:96:f0:4a:31:65:60:d8:10:fc:36:
         44:8a:c1:fb:9c:33:75:fe:a6:08:d3:89:81:b0:6f:c3:04:0b:
         a3:04:a1:d1:1c:46:57:41:08:40:b1:38:f9:57:62:97:10:42:
         8e:f3:a7:a8:77:26:71:74:c2:0a:5b:9e:cc:d5:2c:c5:27:c3:
         12:b9:35:d5
```

## ACM Root CAs<a name="ACM-root-CAs"></a>

The public end\-entity certificates issued by ACM derive their trust from the following Amazon root CAs:


****  

|  Distinguished name  | Encryption algorithm | 
| --- | --- | 
|  CN=Amazon Root CA 1,O=Amazon,C=US  | 2048\-bit RSA \(RSA\_2048\) | 
|  CN=Amazon Root CA 2,O=Amazon,C=US  | 4096\-bit RSA \(RSA\_4096\) | 
|  CN=Amazon Root CA 3,O=Amazon,C=US  | Elliptic Prime Curve 256 bit \(EC\_prime256v1\) | 
|  CN=Amazon Root CA 4,O=Amazon,C=US  | Elliptic Prime Curve 384 bit \(EC\_secp384r1\) | 

The default root of trust for ACM\-issued certificates is CN=Amazon Root CA 1,O=Amazon,C=US, which offers 2048\-bit RSA security\. The other roots are reserved for future use\. All of the roots are cross\-signed by the Starfield Services Root Certificate Authority certificate\.

For more information, see [Amazon Trust Services](https://www.amazontrust.com/repository/)\.

## Apex Domain<a name="concept-apex"></a>

See [Domain Names](#concept-dn)\.

## Asymmetric Key Cryptography<a name="concept-asymmetric"></a>

Unlike [Symmetric Key Cryptography](#concept-symmetric), asymmetric cryptography uses different but mathematically related keys to encrypt and decrypt content\. One of the keys is public and is typically made available in an X\.509 v3 certificate\. The other key is private and is stored securely\. The X\.509 certificate binds the identity of a user, computer, or other resource \(the certificate subject\) to the public key\. 

ACM certificates are X\.509 SSL/TLS certificates that bind the identity of your website and the details of your organization to the public key that is contained in the certificate\. ACM uses the your customer master key \(CMK\) to encrypt the private key\. For more information, see [ACM Private Key Security](data-protection.md#kms)\.

## Certificate Authority<a name="concept-ca"></a>

A certificate authority \(CA\) is an entity that issues digital certificates\. Commercially, the most common type of digital certificate is based on the ISO X\.509 standard\. The CA issues signed digital certificates that affirm the identity of the certificate subject and bind that identity to the public key contained in the certificate\. A CA also typically manages certificate revocation\.

## Certificate Transparency Logging<a name="concept-transparency"></a>

To guard against SSL/TLS certificates that are issued by mistake or by a compromised CA, some browsers require that public certificates issued for your domain be recorded in a certificate transparency log\. The domain name is recorded\. The private key is not\. Certificates that are not logged typically generate an error in the browser\. 

You can monitor the logs to make sure that only certificates you have authorized have been issued for your domain\. You can use a service such as [Certificate Search](https://crt.sh/) to check the logs\. 

Before the Amazon CA issues a publicly trusted SSL/TLS certificate for your domain, it submits the certificate to at least two certificate transparency log servers\. These servers add the certificate to their public databases and return a signed certificate timestamp \(SCT\) to the Amazon CA\. The CA then embeds the SCT in the certificate, signs the certificate, and issues it to you\. The timestamps are included with other X\.509 extensions\. 

```
 X509v3 extensions:

   CT Precertificate SCTs:
     Signed Certificate Timestamp:
       Version   : v1(0)
         Log ID    : BB:D9:DF:...8E:1E:D1:85
         Timestamp : Apr 24 23:43:15.598 2018 GMT
         Extensions: none
         Signature : ecdsa-with-SHA256
                     30:45:02:...18:CB:79:2F
     Signed Certificate Timestamp:
       Version   : v1(0)
         Log ID    : 87:75:BF:...A0:83:0F
         Timestamp : Apr 24 23:43:15.565 2018 GMT
         Extensions: none
         Signature : ecdsa-with-SHA256
                     30:45:02:...29:8F:6C
```

Certificate transparency logging is automatic when you request or renew a certificate unless you choose to opt out\. For more information about opt out, see [Opting Out of Certificate Transparency Logging](acm-bestpractices.md#best-practices-transparency)\. 

## Domain Name System<a name="concept-dns"></a>

The Domain Name System \(DNS\) is a hierarchical distributed naming system for computers and other resources connected to the internet or a private network\. DNS is primarily used to translate textual domain names, such as `aws.amazon.com`, into numerical IP \(Internet Protocol\) addresses of the form `111.122.133.144`\. The DNS database for your domain, however, contains a number of records that can be used for other purposes\. For example, with ACM you can use a CNAME record to validate that you own or control a domain when you request a certificate\. For more information, see [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

## Domain Names<a name="concept-dn"></a>

A domain name is a text string such as `www.example.com` that can be translated by the Domain Name System \(DNS\) into an IP address\. Computer networks, including the internet, use IP addresses rather than text names\. A domain name consists of distinct labels separated by periods: 

**TLD**  
The rightmost label is called the top\-level domain \(TLD\)\. Common examples include `.com`, `.net`, and `.edu`\. Also, the TLD for entities registered in some countries is an abbreviation of the country name and is called a country code\. Examples include `.uk` for the United Kingdom, `.ru` for Russia, and `.fr` for France\. When country codes are used, a second\-level hierarchy for the TLD is often introduced to identify the type of the registered entity\. For example, the `.co.uk` TLD identifies commercial enterprises in the United Kingdom\. 

**Apex domain**  
The apex domain name includes and expands on the top\-level domain\. For domain names that include a country code, the apex domain includes the code and the labels, if any, that identify the type of the registered entity\. The apex domain does not include subdomains \(see the following paragraph\)\. In `www.example.com`, the name of the apex domain is `example.com`\. In `www.example.co.uk`, the name of the apex domain is `example.co.uk`\. Other names that are often used instead of apex include base, bare, root, root apex, or zone apex\. 

**Subdomain**  
Subdomain names precede the apex domain name and are separated from it and from each other by a period\. The most common subdomain name is `www`, but any name is possible\. Also, subdomain names can have multiple levels\. For example, in `jake.dog.animals.example.com`, the subdomains are `jake`, `dog`, and `animals` in that order\. 

**FQDN**  
A fully qualified domain name \(FQDN\) is the complete DNS name for a computer, website, or other resource connected to a network or to the internet\. For example `aws.amazon.com` is the FQDN for Amazon Web Services\. An FQDN includes all domains up to the top–level domain\. For example, `[subdomain1].[subdomain2]...[subdomainn].[apex domain].[top–level domain]` represents the general format of an FQDN\.

**PQDN**  
A domain name that is not fully qualified is called a partially qualified domain name \(PQDN\) and is ambiguous\. A name such as `[subdomain1.subdomain2.]` is a PQDN because the root domain cannot be determined\. 

**Registration**  
The right to use a domain name is delegated by domain name registrars\. Registrars are typically accredited by the Internet Corporation for Assigned Names and Numbers \(ICANN\)\. In addition, other organizations called registries maintain the TLD databases\. When you request a domain name, the registrar sends your information to the appropriate TLD registry\. The registry assigns a domain name, updates the TLD database, and publishes your information to WHOIS\. Typically, domain names must be purchased\. 

## Encryption and Decryption<a name="concept-encrypt"></a>

Encryption is the process of providing data confidentiality\. Decryption reverses the process and recovers the original data\. Unencrypted data is typically called plaintext whether it is text or not\. Encrypted data is typically called ciphertext\. HTTPS encryption of messages between clients and servers uses algorithms and keys\. Algorithms define the step\-by\-step procedure by which plaintext data is converted into ciphertext \(encryption\) and ciphertext is converted back into the original plaintext \(decryption\)\. Keys are used by algorithms during the encryption or decryption process\. Keys can be either private or public\.

## Fully Qualified Domain Name \(FQDN\)<a name="concept-fqdn"></a>

See [Domain Names](#concept-dn)\.

## Public Key Infrastructure<a name="concept-pki"></a>

A public key infrastructure \(PKI\) consists of hardware, software, people, policies, documents, and procedures that are needed to create, issue, manage, distribute, use, store, and revoke digital certificates\. PKI facilitates the secure transfer of information across computer networks\. 

## Root Certificate<a name="concept-root"></a>

A certificate authority \(CA\) typically exists within a hierarchical structure that contains multiple other CAs with clearly defined parent\-child relationships between them\. Child or subordinate CAs are certified by their parent CAs, creating a certificate chain\. The CA at the top of the hierarchy is referred to as the root CA, and its certificate is called the root certificate\. This certificate is typically self\-signed\. 

## Secure Sockets Layer \(SSL\)<a name="concept-ssl"></a>

Secure Sockets Layer \(SSL\) and Transport Layer Security \(TLS\) are cryptographic protocols that provide communication security over a computer network\. TLS is the successor of SSL\. They both use X\.509 certificates to authenticate the server\. Both protocols negotiate a symmetric key between the client and the server that is used to encrypt data flowing between the two entities\. 

## Secure HTTPS<a name="concept-https"></a>

HTTPS stands for HTTP over SSL/TLS, a secure form of HTTP that is supported by all major browsers and servers\. All HTTP requests and responses are encrypted before being sent across a network\. HTTPS combines the HTTP protocol with symmetric, asymmetric, and X\.509 certificate\-based cryptographic techniques\. HTTPS works by inserting a cryptographic security layer below the HTTP application layer and above the TCP transport layer in the Open Systems Interconnection \(OSI\) model\. The security layer uses the Secure Sockets Layer \(SSL\) protocol or the Transport Layer Security \(TLS\) protocol\. 

## SSL Server Certificates<a name="concept-sslcert"></a>

HTTPS transactions require server certificates to authenticate a server\. A server certificate is an X\.509 v3 data structure that binds the public key in the certificate to the subject of the certificate\. An SSL/TLS certificate is signed by a certificate authority \(CA\) and contains the name of the server, the validity period, the public key, the signature algorithm, and more\. 

## Symmetric Key Cryptography<a name="concept-symmetric"></a>

Symmetric key cryptography uses the same key to both encrypt and decrypt digital data\. See also [Asymmetric Key Cryptography](#concept-asymmetric)\. 

## Transport Layer Security \(TLS\)<a name="concept-tls"></a>

See [Secure Sockets Layer \(SSL\)](#concept-ssl)\.

## Trust<a name="concept-trust"></a>

In order for a web browser to trust the identity of a website, the browser must be able to verify the website's certificate\. Browsers, however, trust only a small number of certificates known as CA root certificates\. A trusted third party, known as a certificate authority \(CA\), validates the identity of the website and issues a signed digital certificate to the website's operator\. The browser can then check the digital signature to validate the identity of the website\. If validation is successful, the browser displays a lock icon in the address bar\.