# Managing Tags<a name="tags-manage"></a>

You can add, edit, and delete tags by using the AWS Management Console, the AWS Command Line Interface, or the AWS Certificate Manager API\. 

## Managing Tags \(Console\)<a name="tags-manage-console"></a>

You can use the AWS Management Console to add, delete, or edit tags\. You can also display tags in columns\. 

### Adding a Tag \(Console\)<a name="tags-manage-add-console"></a>

Use the following procedure to add tags by using the ACM console\. 

**To add a tag to a certificate \(console\)**

1. Sign into the AWS Management Console and open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. 

1. Choose the arrow next to the certificate that you want to tag\. 

1. In the details pane, scroll down to **Tags**\.

1. Choose **Edit** and **Add Tag**\.

1. Type a key and a value for the tag\.

1. Choose **Save**\.

### Deleting a Tag \(Console\)<a name="tags-manage-delete-console"></a>

Use the following procedure to delete tags by using the ACM console\. 

**To delete a tag \(console\)**

1. Sign into the AWS Management Console and open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. 

1. Choose the arrow next to the certificate with a tag that you want to delete\. 

1. In the details pane, scroll down to **Tags**\.

1. Choose **Edit**\.

1. Choose the **X** next to the tag you want to delete\. 

1. Choose **Save**\.

### Editing a Tag \(Console\)<a name="tags-manage-edit-console"></a>

Use the following procedure to edit tags by using the ACM console\.

**To edit a tag \(console\)**

1. Sign into the AWS Management Console and open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. 

1. Choose the arrow next to certificate you want to edit\. 

1. In the details pane, scroll down to **Tags**\.

1. Choose **Edit**\.

1. Modify the key or value of the tag you want to change\.

1. Choose **Save**\.

### Showing Tags in Columns \(Console\)<a name="tags-manage-show-console"></a>

Use the following procedure to show tags in columns in the ACM console\.

**To display tags in columns \(console\)**

1. Sign into the AWS Management Console and open the AWS Certificate Manager console at [https://console\.aws\.amazon\.com/acm/home](https://console.aws.amazon.com/acm/home)\. 

1. Choose the tags that you want to display as columns by choosing the gear icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/acm/latest/userguide/images/acm-gear-icon-console.png) in the upper right corner of the console\. 

1. Select the check box beside the tag that you want to display in a column\.

## Managing Tags \(AWS Command Line Interface\)<a name="tags-manage-cli"></a>

Refer to the following topics to learn how to add, list, and delete tags by using the AWS CLI\.
+  [add\-tags\-to\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/add-tags-to-certificate.html) 
+  [list\-tags\-for\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/list-tags-for-certificate.html) 
+  [remove\-tags\-from\-certificate](https://docs.aws.amazon.com/cli/latest/reference/acm/remove-tags-from-certificate.html) 

## Managing Tags \(AWS Certificate Manager API\)<a name="tags-manage-api"></a>

Refer to the following topics to learn how to add, list, and delete tags by using the API\.
+  [AddTagsToCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html) 
+  [ListTagsForCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html) 
+  [RemoveTagsFromCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html) 