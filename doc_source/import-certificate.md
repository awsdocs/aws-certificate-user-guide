# Importing Certificates into AWS Certificate Manager<a name="import-certificate"></a>

In addition to requesting SSL/TLS certificates provided by AWS Certificate Manager \(ACM\), you can import certificates that you obtained outside of AWS\. You might do this because you already obtained a certificate from a third\-party issuer, or because the certificates provided by ACM do not meet your requirements\.

After you import an SSL/TLS certificate obtained outside of AWS and have associated it with services integrated with ACM, you can reimport that certificate while preserving its associations\. Multiple certificates with the same domain name can be imported, but they must be imported one at a time\.

After you import a certificate, you can use it with the [AWS services that are integrated with ACM](acm-services.md)\. The certificates that you import work the same as those provided by ACM, with one important exception: ACM does not provide [managed renewal](managed-renewal.md) for imported certificates\. 

**Important**  
 You are responsible for monitoring the expiration date of your imported certificates and for renewing them before they expire\. If you import a new certificate with the same ARN as the expiring certificate, the new certificate replaces the old one\. In addition, ACM associates the new certificate with the same services and resources as the old certificate\. 

**Important**  
 We recommend that you do not pin an ACM certificate\. For more information, see [Certificate Pinning](acm-bestpractices.md#best-practices-pinning) and [Troubleshoot Certificate Pinning Problems](troubleshooting-pinning.md)\.

To renew an imported certificate, you can obtain a new certificate from your certificate issuer and then import it to ACM, or you can [request a new certificate](gs-acm-request-public.md) from ACM\.

All certificates in ACM are regional resources, including the certificates that you import\. To use the same certificate with Elastic Load Balancing load balancers in different AWS regions, you must import the certificate into each region where you want to use it\. To use a certificate with Amazon CloudFront, you must import it into the US East \(N\. Virginia\) region\. For more information, see [Supported Regions](acm-regions.md)\.

For information about how to import certificates into ACM, see the following topics\. If you encounter problems importing a certificate, see [Troubleshoot Certificate Import Problems](troubleshoot-import.md)\.

**Topics**
+ [Prerequisites for Importing Certificates](import-certificate-prerequisites.md)
+ [Certificate and Key Format for Importing](import-certificate-format.md)
+ [Import a Certificate](import-certificate-api-cli.md)
+ [Reimport a Certificate](import-reimport.md)