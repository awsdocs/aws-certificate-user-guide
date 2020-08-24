# Listing Certificates Managed by ACM<a name="gs-acm-list"></a>

You can use the ACM console or AWS CLI to list the certificates managed by ACM

**Topics**
+ [List Certificates \(Console\)](#gs-acm-list-console)
+ [List Certificates \(CLI\)](#gs-acm-list-cli)

## List Certificates \(Console\)<a name="gs-acm-list-console"></a>

### Display Certificate Information<a name="gs-acm-list-console-display"></a>

Each certificate occupies a row in the console\. By default, the following columns are displayed for each certificate: 
+ **Domain Name** – The fully qualified domain name for the certificate\.
+ **Additional Names** – Additional names that are supported by this certificate\.
+ **Status** – Certificate status\. This can be any of the following values:
  + Pending validation
  + Issued
  + Inactive
  + Expired
  + Revoked
  + Failed
  + Timed out
+ **In Use?** – Whether the ACM certificate is actively associated with an AWS service such as Elastic Load Balancing or CloudFront\. The value can be **No** or **Yes**\.

### Customize the Console Display<a name="gs-acm-list-console-customize"></a>

You can select the columns that you want to display by choosing the gear icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-gear-icon-console.png)\) in the upper right corner of the console\. You can select from among the following columns\. 

![\[Certificate columns.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-show-columns-console.png)

## List Certificates \(CLI\)<a name="gs-acm-list-cli"></a>

You can use the [list\-certificates](https://docs.aws.amazon.com/cli/latest/reference/acm/list-certificates.html) command to list your ACM\-managed certificates\. 

```
aws acm list-certificates --max-items 10
```

The `list-certificates` command outputs the following information\.

```
{
    "CertificateSummaryList": [
        {
            "CertificateArn": "arn:aws:acm:region:account:certificate/123456789012-1234-1234-1234-12345678",
            "DomainName": "example.com"
        },
        {
            "CertificateArn": "arn:aws:acm:region:account:certificate/123456789012-1234-1234-1234-12345678",
            "DomainName": "mydomain.com"
        }
    ]
}
```

By default, only certificates that are supported by [Services Integrated with AWS Certificate Manager](acm-services.md) are listed\. That is, only certificates with **keyTypes** `RSA_1024` or `RSA_2048` and with at least one specified domain are returned\. To see other certificates that you control, such as domainless certificates or certificates using a different algorithm or bit size, provide the `--includes` parameter as shown in the following example\. The parameter allows you to specify a member of the [Filters](https://docs.aws.amazon.com/acm/latest/APIReference/API_Filters.html) structure\. 

```
aws acm list-certificates --max-items 10 --includes keyTypes=RSA_4096
```