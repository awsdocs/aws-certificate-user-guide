# Concepts<a name="acm-concepts"></a>

This section introduces basic terms and concepts related to AWS Certificate Manager \(ACM\)\.

**Certificate Authority**  
A certificate authority \(CA\) is an entity that issues digital certificates\. Commercially, the most common type of digital certificate is based on the ISO X\.509 standard\. The CA issues signed digital certificates that affirm the identity of the certificate subject and bind that identity to the public key contained in the certificate\. The CA also typically manages certificate revocation\.

**Domain Name System**  
The Domain Name System \(DNS\) is a hierarchical distributed naming system for computers and other resources connected to the internet or a private network\. DNS is primarily used to translate textual domain names, such as http://aws\.amazon\.com, into numerical IP \(Internet Protocol\) addresses\. The DNS database for your domain, however, contains a number of records that can be used for other purposes\. For example, with ACM you can use a CNAME record to validate that you own or control a domain when you request a certificate\. For more information, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**Domain Name**  
A fully qualified domain name \(FQDN\) is the complete, human\-readable name for a computer or other resource connected to the internet\. For example aws\.amazon\.com is the FQDN for Amazon Web Services\. An FQDN is made up of various parts\. In the preceding example, *aws* is the name of the host located in the domain amazon\.com, and *\.com* is called the top\-level domain\. The \.com suffix is generally used to represent commercial activity\. There are many different top\-level domains, such as \.net and \.edu\.

**Encryption and Decryption**  
Encryption is the process of providing data confidentiality\. Decryption reverses the process and recovers the original data\. Unencrypted data is typically called plaintext whether it is text or not\. Encrypted data is typically called ciphertext\. HTTPS encryption of messages between clients and servers uses algorithms and keys\. Algorithms define the step\-by\-step procedure by which plaintext data is converted into ciphertext \(encryption\) and ciphertext is converted back into the original plaintext \(decryption\)\. Keys are used by algorithms during the encryption or decryption process\. Keys can be either private or public\.

**Fully Qualified Domain Name \(FQDN\)**  
See [Domain Name](#concept-dn)\.

**Public Key Infrastructure**  
A public key infrastructure \(PKI\) consists of hardware, software, people, policies, documents, and procedures that are needed to create, issue, manage, distribute, use, store, and revoke digital certificates\. PKI facilitates the secure transfer of information across computer networks\.

**Root Certificate**  
A certificate authority typically exists within a hierarchical structure that contains multiple CAs with clearly defined parent\-child relationships between them\. Child subordinate CAs are certified by their parent CAs, creating a certificate chain\. The CA at the top of the hierarchy is referred to as the root CA, and its certificate is called the root certificate\. This certificate is typically self\-signed\.

**Secure Sockets Layer \(SSL\)**  
Secure Sockets Layer \(SSL\) and Transport Layer Security \(TLS\) are cryptographic protocols that provide communication security over a computer network\. TLS is the successor of SSL\. They both use X\.509 certificates to authenticate the server\. Both protocols negotiate a symmetric key between the client and the server that is used to encrypt data flowing between the two entities\.

**Secure HTTPS**  
HTTPS stands for HTTP over SSL/TLS, a secure form of HTTP that is supported by all major browsers and servers\. All HTTP requests and responses are encrypted before being sent across a network\. HTTPS combines the HTTP protocol with symmetric, asymmetric, and X\.509 certificate\-based cryptographic techniques\. HTTPS works by inserting a cryptographic security layer below the HTTP application layer and above the TCP transport layer in the Open Systems Interconnection \(OSI\) model\. The security layer uses the Secure Sockets Layer \(SSL\) protocol or the Transport Layer Security \(TLS\) protocol\.

**SSL Server Certificates**  
HTTPS transactions require server certificates to authenticate a server\. A server certificate is an X\.509 v3 data structure that binds the public key in the certificate to the subject of the certificate\. An SSL/TLS certificate is signed by a certificate authority \(CA\) and contains the name of the server, the validity period, the public key, the signature algorithm, and more\.

**Symmetric Key Cryptography**  
Symmetric key cryptography uses the same key to both encrypt and decrypt digital data\.

**Transport Layer Security \(TLS\)**  
See [Secure Sockets Layer \(SSL\)](#concept-ssl)\.

**Trust**  
In order for a web browser to trust the identity of a website, the browser must be able to verify the website's certificate\. Browsers, however, trust only a small number of certificates known as CA root certificates\. A trusted third party, known as a certificate authority \(CA\), validates the identity of the website and issues a signed digital certificate to the website's operator\. The browser can then check the digital signature to validate the identity of the website\. If validation is successful, the browser displays a lock icon in the address bar\.