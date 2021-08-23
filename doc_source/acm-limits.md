# Quotas<a name="acm-limits"></a>

The following AWS Certificate Manager \(ACM\) service quotas apply to each AWS region per each AWS account\. To request quota increases, create a case at the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. 

**Note**  
New AWS accounts may start with quotas that are lower than those that are described here\. If you unexpectedly hit a quota on a new account, log this with the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm) as a **quota\-increase request**\.

## General quotas<a name="general-limits"></a>

**Topics**


****  

| Item | Default quota | 
| --- | --- | 
| Number of ACM certificatesNew AWS accounts may start with a quota lower than the maximum\. Expired and revoked certificates continue to count toward the total\.Certificates signed by a CA from ACM Private CA do not count toward this total\. | 2500 | 
| Number of ACM certificates per year \(last 365 days\)You can request up to twice your quota of ACM certificates per year, region, and account\. For example, if your quota is 2,500, you can request up to 5,000 ACM certificates per year in a given region and account\. You can only have 2,500 certificates at any given time\. To request 5,000 certificates in a year, you must delete 2,500 during the year to stay within the quota\. If you need more than 2,500 certificates at any given time, you must contact the **[AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)**\. Certificates signed by a CA from ACM Private CA do not count toward this total\. | Twice your account quota | 
| Number of imported certificates | 2500 | 
| Number of imported certificates per year \(last 365 days\) | Twice your account quota | 
| Number of domain names per ACM certificateThe default quota is 10 domain names for each ACM certificate\. Your quota may be greater\. The first domain name that you submit is included as the subject common name \(CN\) of the certificate\. All names are included in the Subject Alternative Name extension\. You can request up to 100 domain names\. To request an increase in your quota, create a case at the [ AWS Support Center ](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-acm)\. Before creating a case, however, make sure you understand how adding more domain names can create more administrative work for you if you use email validation\. For more information, see [Domain validation](acm-bestpractices.md#best-practices-validating)\. The quota for the number of domain names per ACM certificate applies only to certificates that are provided by ACM\. This quota does not apply to certificates that you import into ACM\. The following sections apply only to ACM certificates\. | 10 | 
| Number of Private CAsACM is integrated with AWS Certificate Manager Private Certificate Authority \(ACM Private CA\)\. You can use the ACM console, AWS CLI, or ACM API to request private certificates from an existing private certificate authority \(CA\) hosted by ACM Private CA\. These certificates are managed within the ACM environment and have the same restrictions as public certificates issued by ACM\. For more information, see [Requesting a private certificate](gs-acm-request-private.md)\. You can also issue private certificates by using the standalone ACM Private CA service\. For more information, see [Issue a Private End\-Entity Certificate](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaIssueCert.html)\.A private CA that has been deleted will count towards your quota until the end of its restoration period\. For more information, see [Deleting Your Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PCADeleteCA.html)\. | 200 | 
| Number of Private Certificates per CA \(lifetime\) | 1,000,000 | 

## API rate quotas<a name="api-rate-limits"></a>

The following quotas apply to the ACM API for each region and account\. ACM throttles API requests at different quotas depending on the API operation\. Throttling means that ACM rejects an otherwise valid request because the request exceeds the operation's quota for the number of requests per second\. When a request is throttled, ACM returns a `ThrottlingException` error\. The following table lists each API operation and the quota at which ACM throttles requests for that operation\. 

**Requests\-per\-second quota for each ACM API operation**


****  

| API call | Requests per second | 
| --- | --- | 
|  `AddTagsToCertificate`  |  5  | 
|  `DeleteCertificate`  |  10  | 
|  `DescribeCertificate`  |  10  | 
|  `ExportCertificate`  |  5  | 
|  `ImportCertificate`  |  1  | 
|  `ListCertificates`  |  5  | 
|  `ListTagsForCertificate`  |  10  | 
|  `RemoveTagsFromCertificate`  |  5  | 
|  `RequestCertificate`  |  5  | 
|  `ResendValidationEmail`  |  1  | 



For more information, see [AWS Certificate Manager API Reference](https://docs.aws.amazon.com/acm/latest/APIReference/)\.