# ACM Certificate Characteristics<a name="acm-certificate"></a>

Certificates provided by ACM have the characteristics described on this page\.

**Note**  
These characteristics apply only to certificates provided by ACM\. They might not apply to certificates that you import into ACM\.

**Domain Validation \(DV\)**  
ACM Certificates are domain validated\. That is, the subject field of an ACM Certificate identifies a domain name and nothing more\. When you request an ACM Certificate, you must validate that you own or control all of the domains that you specify in your request\. You can validate ownership by using email or DNS\. For more information, see [Use Email to Validate Domain Ownership](gs-acm-validate-email.md) and [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\.

**Validity Period**  
The validity period for ACM Certificates is currently 13 months\.

**Managed Renewal and Deployment**  
ACM manages the process of renewing ACM Certificates and provisioning the certificates after they are renewed\. Automatic renewal can help you avoid downtime due to incorrectly configured, revoked, or expired certificates\. For more information, see [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md)\.

**Browser and Application Trust**  
ACM Certificates are trusted by all major browsers including Google Chrome, Microsoft Internet Explorer and Microsoft Edge, Mozilla Firefox, and Apple Safari\. Browsers that trust ACM Certificates display a lock icon in their status bar or address bar when connected by SSL/TLS to sites that use ACM Certificates\. ACM Certificates are also trusted by Java\.

**Multiple Domain Names**  
Each ACM Certificate must include at least one fully qualified domain name \(FQDN\), and you can add additional names if you want\. For example, when you are creating an ACM Certificate for `www.example.com`, you can also add the name `www.example.net` if customers can reach your site by using either name\. This is also true of bare domains \(also known as the zone apex or naked domains\)\. That is, you can request an ACM Certificate for www\.example\.com and add the name example\.com\. For more information, see [Request a Certificate](gs-acm-request.md)\.

**Wildcard Names**  
ACM allows you to use an asterisk \(\*\) in the domain name to create an ACM Certificate containing a wildcard name that can protect several sites in the same domain\. For example, `*.example.com` protects `www.example.com` and `images.example.com`\.  
When you request a wildcard certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com** and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. However, you can request a certificate that protects a bare or apex domain and its subdomains by specifying multiple domain names in your request\. For example, you can request a certificate that protects **example\.com** and **\*\.example\.com**\.

**Algorithms**  
Currently, ACM supports the RSA\-2048 encryption and SHA\-256 hashing algorithms\.

**Exceptions**  
Note the following:  

+ ACM does not provide extended validation \(EV\) certificates or organization validation \(OV\) certificates\.

+ ACM does not provide certificates for anything other than the SSL/TLS protocols\.

+ You cannot use ACM Certificates for email encryption\.

+ ACM allows only UTF\-8 encoded ASCII for domain names, including labels that contain "xn\-\-" \(Punycode\)\. ACM does not accept Unicode input \(u\-labels\) for domain names\.

+ ACM does not currently permit you to opt out of managed certificate renewal for ACM Certificates\. Also, managed renewal is not available for certificates that you import into ACM\.

+ You cannot request certificates for Amazon\-owned domain names such as those ending in amazonaws\.com, cloudfront\.net, or elasticbeanstalk\.com\.

+ You cannot download the private key for an ACM Certificate\.

+ You cannot directly install ACM Certificates on your Amazon Elastic Compute Cloud \(Amazon EC2\) website or application\. You can, however, use your certificate with any integrated service\. For more information, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 