# AWS CloudHSM Limits<a name="limits"></a>

The following limits apply to your AWS CloudHSM resources per AWS Region and AWS account\.


**Service Limits**  

| Item | Default Limit | Hard Limit | 
| --- | --- | --- | 
| Clusters | 4 | N/A | 
| HSMs | 6 | N/A | 
| HSMs per cluster | N/A | 28 | 


**System Limits**  

| Item | Hard Limit | 
| --- | --- | 
| Keys per cluster | 3500 | 
| Number of users per cluster | 1024 | 
| Maximum length of a user name | 31 characters | 
| Required password length | 7 to 32 characters | 
| Maximum number of concurrent clients | 1024 | 

To request an increase in the number of clusters or HSMs, use the [service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudhsm) in the AWS Support Center\. 