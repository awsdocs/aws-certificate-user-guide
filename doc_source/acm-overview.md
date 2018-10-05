# What Is AWS Certificate Manager?<a name="acm-overview"></a>

Welcome to the AWS Certificate Manager \(ACM\) service\. ACM handles the complexity of creating and managing public SSL/TLS certificates for your AWS based websites and applications\. You can use public [certificates provided by ACM](gs-acm-request-public.md) \(ACM certificates\) or [ certificates that you import into ACM](import-certificate.md)\. ACM certificates can secure multiple domain names and multiple names within a domain\. You can also use ACM to create wildcard SSL certificates that can protect an unlimited number of subdomains\. 

ACM is tightly linked with AWS Certificate Manager Private Certificate Authority\. You can use ACM PCA to create a private certificate authority \(CA\) and then use ACM to issue private certificates\. These are SSL/TLS X\.509 certificates that identify users, computers, applications, services, servers, and other devices internally\. Private certificates cannot be publicly trusted\. For more information about ACM PCA, see the [AWS Certificate Manager Private Certificate Authority User Guide](https://docs.aws.amazon.com/acm-pca/latest/userguide/)\. Private certificates issued by using ACM are much like public ACM certificates\. They have similar benefits and restrictions\. The benefits include managing the private keys associated with the certificate, renewing certificates, and enabling you to use the console to deploy your private certificate with integrated services\. For more information about the restrictions associated with using ACM, see [Request a Private Certificate](gs-acm-request-private.md)\. You can also use ACM to export a private certificate and encrypted private key to use anywhere\. For more information, see [Export a Private Certificate](gs-acm-export-private.md)\. For information about the benefits of using ACM PCA as a standalone service to issue private certificates, see the introduction in the [ACM PCA User Guide](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\. 

**Note**  
You cannot install public ACM certificates directly on your website or application\. You must install your certificate by using one of the services integrated with ACM and ACM PCA\. For more information about these services, see [Services Integrated with AWS Certificate Manager](acm-services.md)\. 

**Topics**
+ [Concepts](acm-concepts.md)
+ [ACM Certificate Characteristics](acm-certificate.md)
+ [Supported Regions](acm-regions.md)
+ [Integrated Services](acm-services.md)
+ [Site Seals and Trust Logos](acm-siteseal.md)
+ [Limits](acm-limits.md)
+ [Best Practices](acm-bestpractices.md)
+ [Pricing](acm-billing.md)