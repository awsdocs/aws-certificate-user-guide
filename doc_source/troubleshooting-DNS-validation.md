# Troubleshoot DNS validation problems<a name="troubleshooting-DNS-validation"></a>

Consult the following guidance if you are having trouble validating a certificate with DNS\.

**Tip**  
The first step in DNS troubleshooting is to check the current status of your domain with tools such as the following:  
**dig** — [Linux](https://linux.die.net/man/1/dig), [Windows](https://help.dyn.com/how-to-use-binds-dig-tool/)
**nslookup** — [Linux](https://linux.die.net/man/1/nslookup), [Windows](https://linux.die.net/man/1/nslookup)
**whois** — [Linux](https://linux.die.net/man/1/whois), [Windows](https://docs.microsoft.com/en-us/sysinternals/downloads/whois)

**Topics**
+ [Underscores prohibited by DNS provider](#underscores-prohibited)
+ [Default trailing period added by DNS provider](#troubleshooting-trailing-period)
+ [DNS validation on GoDaddy fails](#troubleshooting-DNS-GoDaddy)
+ [Troubleshoot problems with the \.IO top\-level domain](#troubleshoot-iodomains)
+ [ACM Console does not display "Create record in Route 53" button](#troubleshooting-route53-1)
+ [Route 53 validation fails on private \(untrusted\) domains](#troubleshooting-route53-2)
+ [Validation fails for DNS server on a VPN](#troubleshooting-vpn)

## Underscores prohibited by DNS provider<a name="underscores-prohibited"></a>

If your DNS provider prohibits leading underscores in CNAME values, you can remove the underscore from the ACM\-provided value and validate your domain without it\. For example, the CNAME value `_x2.acm-validations.aws` can be changed to `x2.acm-validations.aws` for validation purposes\. However, the CNAME name parameter must always begin with a leading underscore\.

You can use either of the values on the right side of the table below to validate a domain\.


|  Name  |  Type  |  Value  | 
| --- | --- | --- | 
|  \_<random value>\.example\.com\.  |  CNAME  |  \_<random value>\.acm\-validations\.aws\.  | 
|  \_<random value>\.example\.com\.  |  CNAME  |  <random value>\.acm\-validations\.aws\.  | 

## Default trailing period added by DNS provider<a name="troubleshooting-trailing-period"></a>

Some DNS providers add by default a trailing period to the CNAME value that you provide\. As a result, adding the period yourself causes an error\. For example, "`<random_value>.acm-validations.aws.`" is rejected while "`<random_value>.acm-validations.aws`" is accepted\.

## DNS validation on GoDaddy or Namecheap fails<a name="troubleshooting-DNS-GoDaddy"></a>

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

## Troubleshoot problems with the \.IO top\-level domain<a name="troubleshoot-iodomains"></a>

The \.IO top\-level domain is assigned to the British Indian Ocean Territory\. Currently, the domain registry does not display your public information from the WHOIS database\. This is true whether you have privacy protection for the domain enabled or disabled\. When a WHOIS lookup is performed, only obfuscated registrar information is returned\. Therefore, ACM is unable to send validation email to the following three registered contact addresses that are usually available in WHOIS\.
+ Domain registrant
+ Technical contact
+ Administrative contact

ACM does, however, send validation email to the following five common system addresses where *your\_domain* is the domain name you entered when you initially requested a certificate and `.io` is the top level domain\.
+ administrator@*your\_domain*\.io
+ hostmaster@*your\_domain*\.io
+ postmaster@*your\_domain*\.io
+ webmaster@*your\_domain*\.io
+ admin@*your\_domain*\.io

To receive validation mail for an \.IO domain, make sure that you have one of the preceding five email accounts enabled\. If you do not, you will not receive validation email and you will not be issued an ACM certificate\.

**Note**  
We recommend that you use DNS validation rather than email validation\. For more information, see [DNS validationDNS validation](dns-validation.md)\. 

## ACM Console does not display "Create record in Route 53" button<a name="troubleshooting-route53-1"></a>

If you select Amazon Route 53 as your DNS provider, AWS Certificate Manager can interact directly with it to validate your domain ownership\. Under some circumstances, the console's **Create record in Route 53** button may not be available when you expect it\. If this happens, check for the following possible causes\.
+ On the **Validation** page, you did not click the down\-arrow next to your domain name\. 
+ You are not using Route 53 as your DNS provider\.
+ You are logged into ACM and Route 53 through different accounts\.
+ You lack IAM permissions to create records in a zone hosted by Route 53\.
+ You or someone else has already validated the domain\.
+ The domain is not publicly addressable\.

## Route 53 validation fails on private \(untrusted\) domains<a name="troubleshooting-route53-2"></a>

Route 53 is exclusively a *public* DNS service\. During DNS validation, ACM searches for a CNAME in a publicly hosted zone\. When it doesn't find one, it times out after 72 hours with a status of **Validation timed out**\. You cannot use it to host DNS records for private domains, such as untrusted domains in your private PKI, or self\-signed certificates\. 

AWS does provide support for publicly untrusted domains through the [ACM Private CA](https://aws.amazon.com/certificate-manager/private-certificate-authority/) service\. 

## Validation fails for DNS server on a VPN<a name="troubleshooting-vpn"></a>

If you locate a DNS server on a VPN and ACM fails to validate a certificate against it, check if the server is publicly accessible\. Public certificate issuance using ACM DNS validation requires that the domain records be resolvable over the public internet\.
