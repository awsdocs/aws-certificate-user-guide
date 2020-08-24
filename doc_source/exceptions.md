# Handling Exceptions<a name="exceptions"></a>

An AWS Certificate Manager command might fail for several reasons\. For information about each exception, see the table below\. 

## Private Certificate Exception Handling<a name="private_certificate_exception_handling"></a>

The following exceptions can occur when you attempt to renew a private PKI certificate issued by ACM Private CA\. 


| ACM failure code | Comment | 
| --- | --- | 
| `PCA_ACCESS_DENIED` | The private CA has not granted ACM permissions\. This triggers a PCA `AccessDeniedException` failure code\. To remedy the problem, grant the necessary permissions to the ACM service principal using the PCA [CreatePermission](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_CreatePermission.html) operation\. | 
|  `PCA_INVALID_DURATION`  |  The validity period of the requested certificate excedes the validity period of the issuing private CA\. This triggers a PCA `ValidationException` failure code\. To remedy the problem, [install a new CA certificate](https://docs.aws.amazon.com/acm-pca/latest/userguide/PCACertInstall.html) with an appropriate validity period\.  | 
| `PCA_INVALID_STATE` |  The private CA being called is not in the correct state to perform the requested ACM operation\. This triggers a PCA `InvalidStateException` failure code\.  Resolve the issue as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/acm/latest/userguide/exceptions.html)  | 
| `PCA_LIMIT_EXCEEDED` |  The private CA has reached an issuance quota\. This triggers a PCA `LimitExceededException` failure code\. Try repeating your request before proceeding with this help\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/) to request a quota increase\. | 
| `PCA_REQUEST_FAILED` | A network or system error occurred\. This triggers a PCA `RequestFailedException` failure code\. Try repeating your request before proceeding with this help\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\. | 
| `PCA_RESOURCE_NOT_FOUND` |  The private CA has been permanently deleted\. This triggers a PCA `ResourceNotFoundException` failure code\. Verify that you used the correct ARN\. If that fails, you won't be able to use this CA\. To remedy the problem, [create a new CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreateCa.html)\.  | 
| SLR\_NOT\_FOUND | In order to renew a certificate signed by a private CA that resides in another account, ACM requires a Service Linked Role \(SLR\) on the account where the certificate resides\. If you need to recreate a deleted SLR, see [Creating the SLR for ACM](acm-slr.md#create-slr)\. | 