# What Is AWS Certificate Manager?<a name="acm-overview"></a>

AWS Certificate Manager \(ACM\) handles the complexity of creating, storing, and renewing public and private SSL/TLS X\.509 certificates and keys that protect your AWS websites and applications\. You can provide certificates for supported AWS services either by issuing them directly with ACM or by importing third\-party certificates into the ACM management system\. ACM certificates can secure singular domain names, multiple specific domain names, wildcard domains, or combinations of these\. ACM wildcard certificates can protect an unlimited number of subdomains\. You can also export a private ACM certificate and encrypted private key to use anywhere\. For more information about exporting, see [Exporting a Private Certificate](gs-acm-export-private.md)\.

## Is ACM the Right Service for Me?<a name="service-options"></a>

AWS offers two options to customers deploying managed X\.509 certificates\. Choose the best one for your needs\.

1. **AWS Certificate Manager \(ACM\)**—This service is for enterprise customers who need a secure web presence using TLS\. ACM certificates are deployed through Elastic Load Balancing, Amazon CloudFront, Amazon API Gateway, and other [integrated services](acm-services.md)\. The most common application of this kind is a secure public website with significant traffic requirements\. ACM also simplifies security management by automating the renewal of expiring certicates\. *You are in the right place for this service\.*

1. **ACM Private CA**—This service is for enterprise customers building a public key infrastructure \(PKI\) inside the AWS cloud and intended for private use within an organization\. With ACM Private CA, you can create your own certificate authority \(CA\) hierarchy and issue certificates with it for authenticating users, computers, applications, services, servers, and other devices\. Certificates issued by a private CA cannot be used on the internet\. For more information, see the [ACM Private CA User Guide](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\.

**Topics**

[Concepts](acm-concepts.md)

[ACM Certificate Characteristics](acm-certificate.md)

[Supported Regions](acm-regions.md)

[Services Integrated with AWS Certificate Manager](acm-services.md)

[Site Seals and Trust Logos](acm-siteseal.md)

[API Rate Quotas](acm-limits.md#api-rate-limits)

[Best Practices](acm-bestpractices.md)

[Pricing for AWS Certificate Manager](acm-billing.md)