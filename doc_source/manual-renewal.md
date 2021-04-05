# Testing the managed renewal of your private PKI certificates<a name="manual-renewal"></a>

You can use the ACM API or AWS CLI to manually test the configuration of your ACM managed renewal workflow\. By doing so, you can confirm that your certificates will be renewed automatically by ACM prior to expiration\.

**Note**  
You can only test the renewal of certificates issued by ACM Private CA\.

When you use API actions or CLI commands described below, ACM attempts to renew the certificate\. If the renewal succeeds, ACM updates the certificate metadata displayed in the management console or in API output\. If the certificate is associated with an ACM [integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html), the new certificate is deployed and a renewal event is generated in Amazon CloudWatch Events\. If the renewal fails, ACM returns a error and suggests remedial action\. \(You can view this information using the [describe\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) command\.\) If the certificate is not deployed through an integrated service, you still need to export it and manually install it on your resource\. 

**Important**  
In order to renew your ACM Private CA certificates with ACM, you must first grant the ACM service principal permissions to do so\. For more information, see [Assigning Certificate Renewal Permissions to ACM](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaPermissions.html)\.

**To manually test certificate renewal \(AWS CLI\)**

1. Use the [renew\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/renew-certificate.html) command to renew a private exported certificate\.

   ```
   aws acm renew-certificate --certificate-arn arn:aws:acm-pca:region:account:\
   certificate/12345678-1234-1234-1234-123456789012
   ```

1. Then use the [describe\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) command to confirm that the certificate's renewal details have been updated\.

   ```
   aws acm describe-certificate --certificate-arn arn:aws:acm-pca:region:account:\
   certificate/12345678-1234-1234-1234-123456789012
   ```

**To manually test certificate renewal \(ACM API\)**
+ Send a [RenewCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RenewCertificate.html) request, specifying the ARN of the private certificate to renew\. Then use the [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) operation to confirm that the certificate's renewal details have been updated\.