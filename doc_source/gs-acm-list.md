# Listing certificates managed by ACM<a name="gs-acm-list"></a>

You can use the ACM console or AWS CLI to list the certificates managed by ACM\.

**Note**  
If you manage 100 or more certificates, we recommend using the AWS CLI option\. 

**To list your certificates using the console**

1. Open the ACM console at [https://console\.aws\.amazon\.com/acm/](https://console.aws.amazon.com/acm/)\.

1. Review the information in the certificate list\. You can navigate through multiple pages of certificates using the page numbers at upper\-right\. Each certificate occupies a row with the following columns displayed by default for each certificate: 
+ **Domain name** – The fully qualified domain name \(FQDN\) for the certificate\.
+ **Type** – The type of certificate\. Possible values are: **Amazon issued** \| **Private** \| **Imported**
+ **Status** – Certificate status\. Possible values are: **Pending validation** \| **Issued** \| **Inactive** \| **Expired** \| **Revoked** \| **Failed** \| **Timed out**
+ **In use?** – Whether the ACM certificate is actively associated with an AWS service such as Elastic Load Balancing or CloudFront\. The value can be **No** or **Yes**\.

**Note**  
You can customize the columns that you want to display, as well as other settings, by choosing the gear icon in the upper\-right corner of the console\. For more information about the available certificate details, see [Describing ACM certificates](gs-acm-describe.md)\.

**To list your certificates using the AWS CLI**

Use the [list\-certificates](https://docs.aws.amazon.com/cli/latest/reference/acm/list-certificates.html) command to list your ACM\-managed certificates as shown in the following example:

```
$ aws acm list-certificates --max-items 10
```

The command returns information similar to the following:

```
{
    "CertificateSummaryList": [
        {
            "CertificateArn": "arn:aws:acm:region:account:certificate/certificate_ID_1",
            "DomainName": "example.com"
        },
        {
            "CertificateArn": "arn:aws:acm:region:account:certificate/certificate_ID_2",
            "DomainName": "mydomain.com"
        }
    ]
}
```

By default, only certificates with **keyTypes** `RSA_1024` or `RSA_2048` and with at least one specified domain are returned\. To see other certificates that you control, such as domainless certificates or certificates using a different algorithm or bit size, provide the `--includes` parameter as shown in the following example\. The parameter allows you to specify a member of the [Filters](https://docs.aws.amazon.com/acm/latest/APIReference/API_Filters.html) structure\. 

```
$ aws acm list-certificates --max-items 10 --includes keyTypes=RSA_4096
```