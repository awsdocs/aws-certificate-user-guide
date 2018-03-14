# Troubleshoot Certificate Pinning Problems<a name="troubleshooting-pinning"></a>

To renew a certificate, ACM generates a new public\-private key pair\. If your application uses [Certificate Pinning](acm-bestpractices.md#best-practices-pinning), sometimes known as SSL pinning, to pin an ACM Certificate, the application might not be able to connect to your domain after AWS renews the certificate\. For this reason, we recommend that you don't pin an ACM Certificate\. If your application must pin a certificate, you can do the following:

+ [Import your own certificate into ACM](import-certificate.md) and then pin your application to the imported certificate\. ACM doesn't provide managed renewal for imported certificates\.

+ Pin your application to an [ Amazon root certificate](https://www.amazontrust.com/repository/)\.