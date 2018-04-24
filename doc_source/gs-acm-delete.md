# Delete ACM–Managed Certificates<a name="gs-acm-delete"></a>

You can use the ACM console or the AWS CLI to delete a certificate\.

**Topics**
+ [Delete Certificates \(Console\)](#gs-acm-delete-console)
+ [Delete Certificates \(CLI\)](#gs-acm-delete-cli)

## Delete Certificates \(Console\)<a name="gs-acm-delete-console"></a>

In the list of certificates, select the check box for the ACM Certificate that you want to delete\. For **Actions**, choose **Delete**\. 

**Note**  
You cannot delete an ACM Certificate that is being used by another AWS service\. To delete a certificate that is in use, you must first remove the certificate association\. 

## Delete Certificates \(CLI\)<a name="gs-acm-delete-cli"></a>

You can use the [delete\-certificate](http://docs.aws.amazon.com/cli/latest/reference/acm/delete-certificate.html) command list the metadata for a certificate\. 

```
aws acm delete-certificate --certificate-arn arn:aws:acm:region:123456789012:certificate/12345678-1234-1234-1234-123456789012
```