# Use DNS to Validate Domain Ownership<a name="gs-acm-validate-dns"></a>

Before the Amazon certificate authority \(CA\) can issue a certificate for your site, AWS Certificate Manager \(ACM\) must verify that you own or control all of the domains that you specified in your request\. You can choose either email validation or DNS validation when you request a certificate\. This topic discusses DNS validation\. For information about email validation, see [Use Email to Validate Domain Ownership](gs-acm-validate-email.md)\. 

**Note**  
Validation applies only to certificates provided by AWS Certificate Manager \(ACM\)\. ACM does not validate domain ownership for imported certificates\. 

The Domain Name System \(DNS\) is a directory service for resources connected to a network\. On the internet, DNS servers are used primarily to translate from domain names to the numerical IP addresses that identify and locate resources such as computers and other devices\. The databases on DNS servers contain domain records that are used for this translation and to enable other functionality\. For example, A records are a type of DNS record used to map domain names to IPV4 addresses\. MX records are used to route email\. NS records list all of the name servers for the domain\. 

ACM uses a CNAME \(Canonical Name\) record to validate that you own or control a domain\. When you choose DNS validation, ACM provides you a CNAME record to insert into your DNS database\. For example, to validate www\.example\.com, you can add a CNAME record to the zone for example\.com\. The record, created by ACM specifically for your domain and your account, contains a name and a value\. The value is an alias that points to a domain that ACM owns and which ACM uses to automatically renew your certificate\. You add the CNAME record to your DNS database only once\. ACM automatically renews your certificate as long as the certificate is in use and your CNAME record remains in place\. In addition, if you use Amazon Route 53 to create your domain, ACM can write the CNAME record for you\. 

DNS validation has a number of advantages over email validation:

+ DNS requires that you create only one CNAME record per domain name\. Email validation sends up to eight email messages per domain name\.

+ You can obtain additional certificates for your FQDN for as long as the DNS record remains in place\. You do not need to get a new CNAME record again\. You cannot, however, obtain additional certificates for any other FQDN in the domain\.

+ ACM automatically renews your ACM Certificates before they expire as long as the certificate is in use and the DNS record is in place\.

+ ACM can add the CNAME record for you if you use Amazon Route 53 to manage your public DNS records\.

+ You can more easily automate the DNS validation process than you can the email validation process\.

 Note however that you may be required to use email validation if you do not have permission to modify the DNS records for your domain\. 

# To use DNS validation:<a name="gs-acm-use-dns"></a>

1. Sign into the AWS Management Console and open the ACM console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. If the introductory page appears, choose **Get Started**\. Otherwise, choose **Request a certificate**\. 

1. On the **Request a certificate** page, type your domain name\. For more information about typing domain names, see [Request a Certificate](gs-acm-request.md)\.

1. To add more domain names to the ACM Certificate, type other names as text boxes open beneath the name you just typed\.

1. Choose **Next**\.

1. Choose **DNS validation**\.

1. Choose **Review and request**\. Verify that the domain name and validation method are correct\.

1. Choose **Confirm and request**\.

1. On the **Validation** page, expand your domain information or choose **Export DNS configuration to a file**\. If you expand your domain information, ACM displays the name and value of the CNAME record you must add to your DNS database to validate that you control the domain\.  
![\[Console shows the CNAME for DNS validation.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm_dns_cname.png)

1. The **Create record in Route 53** button appears if the following conditions are true:

   + You use Amazon Route 53 as your DNS provider\.

   + You are hosting the domain in Amazon Route 53\.

   + You have permission to write to the Amazon Route 53, hosted zone\.

   + Your FQDN has not already been validated\.

   If your FQDN has already been validated or if you don't have permission to write to the Amazon Route 53 hosted zone for the domain name you are requesting, the **Create record in Route 53** button will appear disabled\. For more information about Amazon Route 53 record sets, see [Working with Resource Record Sets](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/rrsets-working-with.html)\. 
**Note**  
Currently, you cannot programmatically request that ACM automatically create your record in Amazon Route 53\. You can, however, make a AWS CLI or API call to Amazon Route 53 to create the record\.

1. Add the record from the console or the exported file to your database\. For more information about adding DNS records, see [Adding a CNAME to Your Database](#dns-add-cname)\. You can choose **Continue** to skip this step\. You can return to it later by opening the certificate request in the console\. 
**Note**  
If your FQDN was validated when you requested a previous certificate and you are requesting another certificate for the same FQDN, you do not need to add another DNS record\.

   The following table shows example CNAME records for five domain names\. The \_*x* values are long random strings generated by ACM\. For example `_3639ac514e785e898d2646601fa951d5.example.com` is representative of a generated name\. Note that the first two \_*x* values in the table are the same\. That is, the random string created by ACM for the wildcard name `*.example.com` is the same as that created for the base domain name `example.com`\.   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html)

1. After updating your DNS configuration, choose **Continue**\. ACM displays a table view that includes all of your certificates\. The certificate you requested and its status is displayed\. After your DNS provider propagates your record update, it can take up to several hours for ACM to validate the domain name and issue the certificate\. During this time, ACM shows the validation status as **Pending validation**\. After validating the domain name, ACM changes the validation status to **Success**\. After AWS issues the certificate, ACM changes the certificate status to **Issued**\. 
**Note**  
 If ACM is not able to validate the domain name within 72 hours from the time it generates a CNAME value for you, ACM changes the certificate status to **Validation timed out**\. The most likely reason for this result is that you did not update your DNS configuration with the value that ACM generated\. To remedy this issue, you must request a new certificate\.   
![\[Console shows the CNAME for DNS validation.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm_dns_table_view.png)

## Adding a CNAME to Your Database<a name="dns-add-cname"></a>

To use DNS validation, you must be able to add a CNAME record to the DNS configuration for your domain\. If Amazon Route 53 is not your DNS provider, contact your provider to find out how to add records\. If Amazon Route 53 is your provider, ACM can create the CNAME record for you as discussed previously in step 9\. If you want to add the record yourself, see [Editing Resource Record Sets](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) in the *Amazon Route 53 Developer Guide*\. 

**Note**  
If you do not have permission to edit your DNS configuration, you must use email validation\.

## Deleting a CNAME from Your Database<a name="dns-delete-cname"></a>

ACM automatically renews your certificate for as long as the certificate is in use and the CNAME record that ACM created for you remains in place in your DNS database\. You can stop automatic renewal by removing the certificate from the AWS service with which it is associated or by deleting the CNAME record\. If Amazon Route 53 is not your DNS provider, contact your provider to find out how to delete the record\. If Amazon Route 53 is your provider, see [Deleting Resource Record Sets](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-deleting.html) in the *Amazon Route 53 Developer Guide*\. For more information about managed certificate renewal, see [Managed Renewal for ACM's Amazon\-Issued Certificates](managed-renewal.md)\. 