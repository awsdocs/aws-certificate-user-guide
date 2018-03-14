# Tag Restrictions<a name="tags-restrictions"></a>

The following basic restrictions apply to ACM Certificate tags:

+ The maximum number of tags per ACM Certificate is 50\.

+ The maximum length of a tag key is 127 characters\.

+ The maximum length of a tag value is 255 characters\.

+ Tag keys and values are case sensitive\.

+ The `aws:` prefix is reserved for AWS use; you cannot add, edit, or delete tags whose key begins with `aws:`\. Tags that begin with `aws:` do not count against your tags\-per\-resource limit\.

+ If you plan to use your tagging schema across multiple services and resources, remember that other services may have other restrictions for allowed characters\. Refer to the documentation for that service\.

+ ACM Certificate tags are not available for use in the AWS Management Console's [Resource Groups and Tag Editor](https://aws.amazon.com/blogs/aws/resource-groups-and-tagging/)\.