# \(Optional\) Configure a CAA Record<a name="setup-caa"></a>

You can optionally configure a Certification Authority Authorization \(CAA\) DNS record to specify that AWS Certificate Manager \(ACM\) is allowed to issue a certificate for your domain or subdomain\. After it validates your domain, ACM checks for the presence of CAA records to make sure it can issue a certificate for you\. You can choose to not configure a CAA record for your domain or leave the record blank if you do not want to enable CAA checking\. A CAA record contains the following data fields: 

**flags**  
Specifies whether the value of the **tag** field is supported by ACM\. Set this value to **0**\.

**tag**  
The **tag** field can be one of the following values\. Note that the **iodef** field is currently ignored\.     
**issue**  
Indicates that the ACM CA that you specify in the **value** field is authorized to issue a certificate for your domain or subdomain\.   
**issuewild**  
Indicates that the ACM CA that you specified in the **value** field is authorized to issue a wildcard certificate for your domain or subdomain\. A wildcard certificate applies to the domain or subdomain and all of its subdomains\. 

**value**  
The value of this field depends on the value of the **tag** field\. You must enclose this value in quotation marks \(""\)\.     
When **tag** is **issue**  
The **value** field contains the CA domain name\. This field can contain the name of a CA other than an Amazon CA\. However, if you do not have a CAA record that specifies one of the following four Amazon CAs, ACM cannot issue a certificate to your domain or subdomain:   
+ amazon\.com
+ amazontrust\.com
+ awstrust\.com
+ amazonaws\.com
The **value** field can also contain a semicolon \(;\) to indicate that no CA should be permitted to issue a certificate for your domain or subdomain\. Use this field if you decide at some point that you no longer want a certificate issued for a particular domain\.  
When **tag** is **issuewild**  
The **value** field is the same as that for when **tag** is **issue** except that the value applies to wildcard certificates\.   
When there is an **issuewild** CAA record present that does not include an ACM CA value, then no wildcards can be issued by ACM\. If there is no **issuewild** present, but there is an **issue** CAA record for ACM, then wildcards may be issued by ACM\. 

**Example CAA Record Examples**  
In the following examples, your domain name comes first followed by the record type \(CAA\)\. The **flags** field is always 0\. The **tags** field can be **issue** or **issuewild**\. If the field is **issue** and you type the domain name of a CA server in the **value** field, the CAA record indicates that your specified server is permitted to issue your requested certificate\. If you type a semicolon ";" in the **value** field, the CAA record indicates that no CA is permitted to issue a certificate\. The configuration of CAA records varies by DNS provider\.   

```
Domain   Record type  Flags  Tag      Value   

example.com.   CAA           0      issue   "SomeCA.com"
example.com.   CAA           0      issue   "amazon.com"
example.com.   CAA           0      issue   "amazontrust.com"
example.com.   CAA           0      issue   "awstrust.com"
example.com.   CAA           0      issue   "amazonaws.com"
example.com    CAA           0      issue   ";"
```

For more information about how to add or modify DNS records, check with your DNS provider\. Route 53 supports CAA records\. If Route 53 is your DNS provider, see [CAA Format](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html#CAAFormat) for more information about creating a record\. 