# Concepts<a name="acm-concepts"></a>

This section introduces basic terms and concepts related to AWS Certificate Manager \(ACM\)\.

**Apex Domain**  <a name="concept-apex"></a>
See [Domain Names](#concept-dn)\.

**Asymmetric Key Cryptography**  <a name="concept-asymmetric"></a>
Unlike [Symmetric Key Cryptography](#concept-symmetric), asymmetric cryptography uses different but mathematically related keys to encrypt and decrypt content\. One of the keys is public and is typically made available in an X\.509 v3 certificate\. The other key is private and is stored securely\. The X\.509 certificate binds the identity of a user, computer, or other resource \(the certificate subject\) to the public key\.   
ACM Certificates are X\.509 SSL/TLS certificates that bind the identity of your website and the details of your organization to the public key that is contained in the certificate\. ACM stores the associated private key in a hardware security module \(HSM\)\. 

**Certificate Authority**  <a name="concept-ca"></a>
A certificate authority \(CA\) is an entity that issues digital certificates\. Commercially, the most common type of digital certificate is based on the ISO X\.509 standard\. The CA issues signed digital certificates that affirm the identity of the certificate subject and bind that identity to the public key contained in the certificate\. A CA also typically manages certificate revocation\.

**Domain Name System**  <a name="concept-dns"></a>
The Domain Name System \(DNS\) is a hierarchical distributed naming system for computers and other resources connected to the internet or a private network\. DNS is primarily used to translate textual domain names, such as `aws.amazon.com`, into numerical IP \(Internet Protocol\) addresses of the form `111.222.333.444`\. The DNS database for your domain, however, contains a number of records that can be used for other purposes\. For example, with ACM you can use a CNAME record to validate that you own or control a domain when you request a certificate\. For more information, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

**Domain Names**  <a name="concept-dn"></a>
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

**Encryption and Decryption**  <a name="concept-encrypt"></a>
Encryption is the process of providing data confidentiality\. Decryption reverses the process and recovers the original data\. Unencrypted data is typically called plaintext whether it is text or not\. Encrypted data is typically called ciphertext\. HTTPS encryption of messages between clients and servers uses algorithms and keys\. Algorithms define the step\-by\-step procedure by which plaintext data is converted into ciphertext \(encryption\) and ciphertext is converted back into the original plaintext \(decryption\)\. Keys are used by algorithms during the encryption or decryption process\. Keys can be either private or public\.

**Fully Qualified Domain Name \(FQDN\)**  <a name="concept-fqdn"></a>
See [Domain Names](#concept-dn)\.

**Public Key Infrastructure**  <a name="concept-pki"></a>
A public key infrastructure \(PKI\) consists of hardware, software, people, policies, documents, and procedures that are needed to create, issue, manage, distribute, use, store, and revoke digital certificates\. PKI facilitates the secure transfer of information across computer networks\.

**Root Certificate**  <a name="concept-root"></a>
A certificate authority \(CA\) typically exists within a hierarchical structure that contains multiple other CAs with clearly defined parent\-child relationships between them\. Child or subordinate CAs are certified by their parent CAs, creating a certificate chain\. The CA at the top of the hierarchy is referred to as the root CA, and its certificate is called the root certificate\. This certificate is typically self\-signed\.

**Secure Sockets Layer \(SSL\)**  <a name="concept-ssl"></a>
Secure Sockets Layer \(SSL\) and Transport Layer Security \(TLS\) are cryptographic protocols that provide communication security over a computer network\. TLS is the successor of SSL\. They both use X\.509 certificates to authenticate the server\. Both protocols negotiate a symmetric key between the client and the server that is used to encrypt data flowing between the two entities\.

**Secure HTTPS**  <a name="concept-https"></a>
HTTPS stands for HTTP over SSL/TLS, a secure form of HTTP that is supported by all major browsers and servers\. All HTTP requests and responses are encrypted before being sent across a network\. HTTPS combines the HTTP protocol with symmetric, asymmetric, and X\.509 certificate\-based cryptographic techniques\. HTTPS works by inserting a cryptographic security layer below the HTTP application layer and above the TCP transport layer in the Open Systems Interconnection \(OSI\) model\. The security layer uses the Secure Sockets Layer \(SSL\) protocol or the Transport Layer Security \(TLS\) protocol\.

**SSL Server Certificates**  <a name="concept-sslcert"></a>
HTTPS transactions require server certificates to authenticate a server\. A server certificate is an X\.509 v3 data structure that binds the public key in the certificate to the subject of the certificate\. An SSL/TLS certificate is signed by a certificate authority \(CA\) and contains the name of the server, the validity period, the public key, the signature algorithm, and more\.

**Symmetric Key Cryptography**  <a name="concept-symmetric"></a>
Symmetric key cryptography uses the same key to both encrypt and decrypt digital data\. See also [Asymmetric Key Cryptography](#concept-asymmetric)\. 

**Transport Layer Security \(TLS\)**  <a name="concept-tls"></a>
See [Secure Sockets Layer \(SSL\)](#concept-ssl)\.

**Trust**  <a name="concept-trust"></a>
In order for a web browser to trust the identity of a website, the browser must be able to verify the website's certificate\. Browsers, however, trust only a small number of certificates known as CA root certificates\. A trusted third party, known as a certificate authority \(CA\), validates the identity of the website and issues a signed digital certificate to the website's operator\. The browser can then check the digital signature to validate the identity of the website\. If validation is successful, the browser displays a lock icon in the address bar\.