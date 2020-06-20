# Troubleshoot Problems with the \.IO Domain<a name="troubleshoot-iodomains"></a>

The \.IO domain is assigned to the British Indian Ocean Territory\. Currently, the domain registry does not display your public information from the WHOIS database\. This is true whether you have privacy protection for the domain enabled or disabled\. When a WHOIS lookup is performed, only obfuscated registrar information is returned\. Therefore, ACM is unable to send validation email to the following three registered contact addresses that are usually available in WHOIS\.
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
We recommend that you use DNS validation rather than email validation\. For more information, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 