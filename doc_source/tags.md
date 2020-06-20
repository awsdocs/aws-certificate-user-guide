# Tagging AWS Certificate Manager Certificates<a name="tags"></a>

A *tag* is a label that you can assign to an ACM certificate\. Each tag consists of a *key* and a *value*\. You can use the AWS Certificate Manager console, AWS Command Line Interface \(AWS CLI\), or ACM API to add, view, or remove tags for ACM certificates\. You can choose which tags to display in the ACM console\.

You can create custom tags that suit your needs\. For example, you could tag multiple ACM certificates with an `Environment = Prod` or `Environment = Beta` tag to identify which environment each ACM certificate is intended for\. The following list includes a few additional examples of other custom tags: 
+ `Admin = Alice`
+ `Purpose = Website`
+ `Protocol = TLS`
+ `Registrar = Route53`

Other AWS resources also support tagging\. You can, therefore, assign the same tag to different resources to indicate whether those resources are related\. For example, you can assign a tag such as `Website = example.com` to the ACM certificate, the load balancer, and other resources used for your example\.com website\. 

**Topics**
+ [Tag Restrictions](tags-restrictions.md)
+ [Managing Tags](tags-manage.md)