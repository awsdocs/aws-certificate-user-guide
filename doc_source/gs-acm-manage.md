# Manage ACM Certificates<a name="gs-acm-manage"></a>

You can manage issued ACM certificates and pending certificate requests from the AWS Management Console or the AWS CLI\. You can also manage the certificates that you have imported into ACM\. 

## Manage ACM Certificates \(Console\)<a name="gs-acm-manage-console"></a>

You can use the ACM console to get information about or delete an ACM Certificate\. If you are using email validation, you can also request that ACM resend the validation email\. 

### Display ACM Certificate Information<a name="gs-acm-manage-console-display"></a>

Each of the ACM Certificates occupies a row in the console\. By default, the following columns are displayed for each certificate: 

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

+ **In Use?** – Whether the ACM Certificate is actively associated with an AWS service such as Elastic Load Balancing or CloudFront\. The value can be **No** or **Yes**\.

### Customize the Console Display<a name="gs-acm-manage-console-customize"></a>

You can select the columns that you want to display by choosing the gear icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-gear-icon-console.png)\) in the upper right corner of the console\. You can select from among the following columns\. 

![\[Certificate columns.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-show-columns-console.png)

### Display Certificate Metadata<a name="gs-acm-manage-console-metadata"></a>

To show ACM Certificate metadata, choose the arrow to the immediate left of the domain name\. The console displays information similar to the following\. 

![\[Certificate columns.\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-metadata-console.png)

### Delete an ACM Certificate<a name="gs-acm-manage-console-delete"></a>

In the list of certificates, select the check box for the ACM Certificate that you want to delete\. For **Actions**, choose **Delete**\. 

**Note**  
You cannot delete an ACM Certificate that is being used by another AWS service\. To delete a certificate that is in use, you must first remove the certificate association\.

### Resend Validation Email \(ACM Certificates\)<a name="gs-acm-manage-console-resend"></a>

You can use email to validate that you own or control a domain\. Each email contains a validation token that you can use to approve a certificate request\. However, because the validation email required for the approval process can be blocked by spam filters or lost in transit, the validation token automatically expires after 72 hours\. If you do not receive the original email or the token has expired, you can request that the email be resent\. To do that, select the check box for the pending ACM Certificate, choose **Actions**, and then choose **Resend validation email**\. If the 72\-hour period has passed and the certificate status has changed to **Timed out**, you cannot resend validation email\.

**Note**  
The preceding information applies only to certificates provided by ACM and only to certificates that use email validation\. Validation email is not required for [certificates that you imported into ACM](import-certificate.md)\.

**Note**  
Resending validation email applies only to certificates that use email validation, not DNS validation\. For more information about DNS domain validation, see [Use DNS to Validate Domain Ownership](gs-acm-validate-dns.md)\. 

## Manage ACM Certificates \(AWS CLI\)<a name="gs-acm-manage-cli"></a>

You can use the AWS CLI to get information about an issued certificate, delete a certificate, or resend validation email\.

### Retrieve ACM Certificate Fields<a name="gs-acm-manage-cli-describe"></a>

You can use the [describe\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/describe-certificate.html) command to retrieve information about a certificate\.

```
aws acm describe-certificate --certificate-arn arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012
```

### Delete an ACM Certificate<a name="gs-acm-manage-cli-delete"></a>

You can use the [delete\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/delete-certificate.html) command to delete a certificate\.

```
aws acm delete-certificate --certificate-arn arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012
```

### Resend Validation Email \(ACM Certificates\)<a name="gs-acm-manage-cli-resend"></a>

You can use the [resend\-validation\-email](http://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command to send validation email again\.

```
aws acm resend-validation-email --certificate-arn arn:aws:acm:us-east-1:account:certificate/12345678-1234-1234-1234-123456789012 --validation-domain example.com
```

**Note**  
The [resend\-validation\-email](http://docs.aws.amazon.com/cli/latest/reference/acm/resend-validation-email.html) command applies only to ACM Certificates for which you are using email validation\. Validation is not required for certificates that you have imported into ACM\.