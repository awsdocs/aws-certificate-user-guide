# Limits<a name="acm-limits"></a>

The following AWS Certificate Manager \(ACM\) limits apply to each AWS region and each AWS account\. To request higher limits, create a case at the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. New AWS accounts might start with limits that are lower than those that are described here\. 


****  

| Item | Default Limit | 
| --- | --- | 
| Number of ACM Certificates | 1000 | 
| Number of ACM Certificates per year \(last 365 days\) | Twice your account limit | 
| Number of imported certificates | 1000 | 
| Number of imported certificates per year \(last 365 days\) | Twice your account limit | 
| Number of domain names per ACM Certificate | 10 | 
| Number of Private CAs | 10 | 
| Number of Private Certificates per CA | 50,000 | 

**Topics**
+ [Number of ACM Certificates per Year \(Last 365 Days\)](#limit-certs-yearly)
+ [Number of Domain Names per ACM Certificate](#limit-domain-names)
+ [Number of Private CAs and Certificates](#limit-private-ca)

## Number of ACM Certificates per Year \(Last 365 Days\)<a name="limit-certs-yearly"></a>

 You can request up to twice your limit of ACM Certificates every year\. For example, if your limit is 1,000, you can request up to 2,000 ACM Certificates a year\. You can only have 1,000 certificates at any given time\. To request 2,000 certificates in a year, you must delete 1,000 during the year to stay within the limit\. If you need more than 1,000 certificates at any given time, you must contact the **AWS Support Center**\. 

**Note**  
Although the preceding table indicates that an account can own up to 1,000 ACM Certificates, new AWS accounts might start with a lower limit\.

## Number of Domain Names per ACM Certificate<a name="limit-domain-names"></a>

The default limit is 10 domain names for each ACM Certificate\. Your limit may be greater\. The first domain name that you submit is included as the subject common name \(CN\) of the certificate\. All names are included in the Subject Alternative Name extension\. 

You can request up to 100 domain names\. To request an increase in your limit, create a case at the [ AWS Support Center ](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. Before creating a case, however, make sure you understand how adding more domain names can create more administrative work for you if you use email validation\. For more information, see [Domain Validation](acm-bestpractices.md#best-practices-validating)\. 

**Note**  
 The limit for the number of domain names per ACM Certificate applies only to certificates that are provided by ACM\. This limit does not apply to certificates that you import into ACM\. The following sections apply only to ACM Certificates\. 

## Number of Private CAs and Certificates<a name="limit-private-ca"></a>

ACM is integrated with ACM PCA\. You can use the ACM console, AWS CLI, or ACM API to request private certificates from an existing private certificate authority \(CA\)\. The certificates are managed within the ACM environment and have the same restrictions as public certificates issued by ACM\. For more information, see [Request a Private Certificate](gs-acm-request-private.md)\. You can also issue private certificates by using the standalone ACM PCA service\. For more information, see [Issue a Private Certificate](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaIssueCert.html)\. You can create 10 private CAs and 50,000 private certificates for each\. 

**Note**  
A private CA that has been deleted will count towards your limit until the end of its restoration period\. For more information, see [Delete Your Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PCADeleteCA.html)\.