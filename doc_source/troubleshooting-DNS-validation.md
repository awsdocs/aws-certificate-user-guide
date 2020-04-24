# Troubleshoot DNS Validation Problems<a name="troubleshooting-DNS-validation"></a>

Consult the following guidance if you are having touble validating a certificate with DNS\.

**Tip**  
The first step in DNS troubleshooting is to check the current status of your domain with tools such as the following:  
**dig** — [Linux](https://linux.die.net/man/1/dig), [Windows](https://help.dyn.com/how-to-use-binds-dig-tool/)
**nslookup** — [Linux](https://linux.die.net/man/1/nslookup), [Windows](https://linux.die.net/man/1/nslookup)
**whois** — [Linux](https://linux.die.net/man/1/whois), [Windows](https://docs.microsoft.com/en-us/sysinternals/downloads/whois)

**Topics**
+ [Troubleshoot Certification Authority Authorization \(CAA\) Problems](#troubleshooting-caa)
+ [Underscores Prohibited by DNS Provider](#underscores-prohibited)
+ [DNS Validation on GoDaddy Fails](#troubleshooting-DNS-GoDaddy)
+ [Troubleshoot Problems with the \.IO Domain](troubleshoot-iodomains.md)
+ [ACM Console Does Not Display "Create record in Route 53" Button](troubleshooting-route53.md)

## Troubleshoot Certification Authority Authorization \(CAA\) Problems<a name="troubleshooting-caa"></a>

You can use CAA DNS records to specify that the Amazon certificate authority \(CA\) can issue ACM certificates for your domain or subdomain\. If you receive an error during certificate issuance that says **One or more domain names have failed validation due to a Certification Authority Authentication \(CAA\) error**, check your CAA DNS records\. If you receive this error after your ACM certificate request has been successfully validated, you must update your CAA records and request a certificate again\. The **value** field in at least one of your CAA records must contain one of the following domain names:
+ amazon\.com
+ amazontrust\.com
+ awstrust\.com
+ amazonaws\.com

If you do not want ACM to perform CAA checking, do not configure a CAA record for your domain or leave your CAA records blank\. For more information about creating a CAA record, see [\(Optional\) Configure a CAA Record](setup-caa.md)\.

## Underscores Prohibited by DNS Provider<a name="underscores-prohibited"></a>

If your DNS provider prohibits leading underscores in CNAME values, you can remove the underscore from the ACM\-provided value and validate your domain without it\. For example, the CNAME value `_x2.acm-validations.aws` can be changed to `x2.acm-validations.aws` for validation purposes\. However, the CNAME name parameter must always begin with a leading underscore\.

You can use either of the values on the right side of the table below to validate a domain\.


|  Name  |  Type  |  Value  | 
| --- | --- | --- | 
|  \_<random value>\.example\.com\.  |  CNAME  |  \_<random value>\.acm\-validations\.aws\.  | 
|  \_<random value>\.example\.com\.  |  CNAME  |  <random value>\.acm\-validations\.aws\.  | 

## DNS Validation on GoDaddy Fails<a name="troubleshooting-DNS-GoDaddy"></a>

DNS validation for domains registered with Godaddy and other registries may fail unless you modify the CNAME values provided by ACM\. Taking example\.com as the domain name, the issued CNAME record has the following form:

```
NAME: _ho9hv39800vb3examplew3vnewoib3u.example.com.
    VALUE: _cjhwou20vhu2exampleuw20vuyb2ovb9.j9s73ucn9vy.acm-validations.aws.
```

You can create a CNAME record compatible with GoDaddy by truncating the apex domain \(including the period\) at the end of the NAME field, as follows:

```
NAME: _ho9hv39800vb3examplew3vnewoib3u
    VALUE: _cjhwou20vhu2exampleuw20vuyb2ovb9.j9s73ucn9vy.acm-validations.aws.
```