# Troubleshoot Certification Authority Authorization \(CAA\) Problems<a name="troubleshooting-caa"></a>

You can use CAA DNS records to specify that the Amazon certificate authority \(CA\) can issue ACM Certificates for your domain or subdomain\. If you receive an error during certificate issuance that says **One or more domain names have failed validation due to a Certification Authority Authentication \(CAA\) error**, check your CAA DNS records\. If you receive this error after your ACM Certificate request has been successfully validated, you must update your CAA records and request a certificate again\. The **value** field in at least one of your CAA records must contain one the following domain names:
+ amazon\.com
+ amazontrust\.com
+ awstrust\.com
+ amazonaws\.com

If you do not want ACM to perform CAA checking, do not configure a CAA record for your domain or leave your CAA records blank\. For more information about creating a CAA record, see [\(Optional\) Configure a CAA Record](setup-caa.md)\.