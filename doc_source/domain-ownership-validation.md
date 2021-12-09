# Validating domain ownership<a name="domain-ownership-validation"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must prove that you own or control all of the domain names that you specify in your request\. You can choose to prove your ownership with either Domain Name System \(DNS\) validation or with email validation at the time you request a certificate\. 

**Note**  
Validation applies only to publicly trusted certificates issued by ACM\. ACM does not validate domain ownership for [imported certificates](import-certificate.md) or for certificates signed by a private CA\.

In general, we recommend using DNS validation over email validation for the following reasons:
+ If you use Amazon Route 53 to manage your public DNS records, you can update your records through ACM directly\.
+ ACM automatically renews DNS\-validated certificates for as long as a certificate remains in use and the DNS record is in place\. 
+ To be renewed, email\-validated certificates require an action by the domain owner\. ACM begins sending renewal notices 45 days before expiration\. These notices go to the domain's WHOIS mailbox addresses and to five common administrator addresses\. The notifications contain a link that the domain owner can click for easy renewal\. Once all listed domains are validated, ACM issues a renewed certificate with the same ARN\.

 If you lack authorization to edit your domain's DNS database, you must use [email validation](email-validation.md) instead\.

Topics

[DNS validationDNS validation](dns-validation.md)

[Email validation](email-validation.md)