# Limits<a name="acm-limits"></a>

The following AWS Certificate Manager \(ACM\) limits apply to each AWS region and each AWS account\. To request higher limits, create a case at the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. New AWS accounts might start with limits that are lower than those that are described here\. 


****  

| Item | Default Limit | 
| --- | --- | 
| Number of ACM Certificates | 100 | 
| Number of imported certificates | 100 | 
| Number of imported certificates per year \(last 365 days\) | 200 | 
| Number of Domain Names per ACM Certificate | 10 | 
| Number of ACM Certificates per Year \(last 365 days\) | Twice your account limit | 

**Note**  
 The limit for the number of domain names per ACM Certificate applies only to certificates that are provided by ACM\. This limit does not apply to certificates that you import into ACM\. The following sections apply only to ACM Certificates\.

## Number of ACM Certificates per Year \(Last 365 Days\)<a name="limit-certs-yearly"></a>

 You can request up to twice your limit of ACM Certificates every year\. For example, if your limit is 25, you can request up to 50 ACM Certificates a year\. If you request 50 certificates, you must delete 25 during the year to stay within your limit\. If you need more than 25 certificates, in this example, you must contact the **AWS Support Center**\. 

**Note**  
Although the preceding table indicates that an account can own up to 100 ACM Certificates, new AWS accounts might start with a lower limit\.

## Number of Domain Names per ACM Certificate<a name="limit-domain-names"></a>

The default limit is 10 domain names for each ACM Certificate\. Your limit may be greater\. The first domain name that you submit is included as the subject common name \(CN\) of the certificate\. All names are included in the Subject Alternative Name extension\. 

You can request up to 100 domain names\. To request an increase in your limit, create a case at the [ AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. Before creating a case, however, make sure you understand how adding more domain names can create more administrative work for you if you use email validation\. For more information, see [Requesting a Limit Increase](acm-bestpractices.md#best-practices-limit-increase)\. 