# ACM Certificate Characteristics<a name="acm-certificate"></a>

Public certificates provided by ACM have the characteristics described on this page\.

**Note**  
These characteristics apply only to certificates provided by ACM\. They might not apply to [certificates that you import into ACM](import-certificate.md)\.

**Domain Validation \(DV\)**  <a name="domain-validation"></a>
ACM certificates are domain validated\. That is, the subject field of an ACM certificate identifies a domain name and nothing more\. When you request an ACM certificate, you must validate that you own or control all of the domains that you specify in your request\. You can validate ownership by using email or DNS\. For more information, see [Using Email to Validate Domain Ownership](gs-acm-validate-email.md) and [Using DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\.

**Validity Period**  <a name="validity"></a>
The validity period for ACM certificates is 13 months \(395 days\)\.

**Managed Renewal and Deployment**  <a name="renewal"></a>
ACM manages the process of renewing ACM certificates and provisioning the certificates after they are renewed\. Automatic renewal can help you avoid downtime due to incorrectly configured, revoked, or expired certificates\. For more information, see [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md)\.

**Browser and Application Trust**  <a name="browser-trust"></a>
ACM certificates are trusted by all major browsers including Google Chrome, Microsoft Internet Explorer and Microsoft Edge, Mozilla Firefox, and Apple Safari\. Browsers that trust ACM certificates display a lock icon in their status bar or address bar when connected by SSL/TLS to sites that use ACM certificates\. ACM certificates are also trusted by Java\.

**Multiple Domain Names**  <a name="multiple-domains"></a>
Each ACM certificate must include at least one fully qualified domain name \(FQDN\), and you can add additional names if you want\. For example, when you are creating an ACM certificate for `www.example.com`, you can also add the name `www.example.net` if customers can reach your site by using either name\. This is also true of bare domains \(also known as the zone apex or naked domains\)\. That is, you can request an ACM certificate for www\.example\.com and add the name example\.com\. For more information, see [Requesting a Public Certificate](gs-acm-request-public.md)\.

**Wildcard Names**  <a name="wildcard"></a>
ACM allows you to use an asterisk \(\*\) in the domain name to create an ACM certificate containing a wildcard name that can protect several sites in the same domain\. For example, `*.example.com` protects `www.example.com` and `images.example.com`\.  
When you request a wildcard certificate, the asterisk \(**\***\) must be in the leftmost position of the domain name and can protect only one subdomain level\. For example, **\*\.example\.com** can protect **login\.example\.com** and **test\.example\.com**, but it cannot protect **test\.login\.example\.com**\. Also note that **\*\.example\.com** protects *only* the subdomains of **example\.com**, it does not protect the bare or apex domain \(**example\.com**\)\. However, you can request a certificate that protects a bare or apex domain and its subdomains by specifying multiple domain names in your request\. For example, you can request a certificate that protects **example\.com** and **\*\.example\.com**\.

**Algorithms**  <a name="algorithms"></a>
A certificate must specify an algorithm and key size\. Currently, the following public key algorithms are supported by ACM:   
+ 2048\-bit RSA \(`RSA_2048`\)
+ 4096\-bit RSA \(`RSA_4096`\)
+ Elliptic Prime Curve 256 bit \(`EC_prime256v1`\)
+ Elliptic Prime Curve 384 bit \(`EC_secp384r1`\)
Note that [integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html) allow only algorithms and key sizes they support to be associated with their resources\. Further, their support differs depending on whether the certificate is imported into IAM or into ACM\. For more information, see the documentation for each service\.   
+ For Elastic Load Balancing, see [ HTTPS Listeners for Your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html)\.
+ For CloudFront, see [ Supported SSL/TLS Protocols and Ciphers](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/secure-connections-supported-viewer-protocols-ciphers.html#secure-connections-supported-ciphers)\.

**Exceptions**  <a name="exceptions"></a>
Note the following:  
+ ACM does not provide extended validation \(EV\) certificates or organization validation \(OV\) certificates\.
+ ACM does not provide certificates for anything other than the SSL/TLS protocols\.
+ You cannot use ACM certificates for email encryption\.
+ ACM allows only UTF\-8 encoded ASCII for domain names, including labels that contain "xn\-\-" \(Punycode\)\. ACM does not accept Unicode input \(u\-labels\) for domain names\.
+ ACM does not currently permit you to opt out of [managed certificate renewal](managed-renewal.md) for ACM certificates\. Also, managed renewal is not available for certificates that you import into ACM\.
+ You cannot request certificates for Amazon\-owned domain names such as those ending in amazonaws\.com, cloudfront\.net, or elasticbeanstalk\.com\.
+ You cannot download the private key for an ACM certificate\.
+ You cannot directly install ACM certificates on your Amazon Elastic Compute Cloud \(Amazon EC2\) website or application\. You can, however, use your certificate with any integrated service\. For more information, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 