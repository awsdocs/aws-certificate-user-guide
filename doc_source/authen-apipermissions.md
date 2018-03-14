# ACM API Permissions: Actions and Resources Reference<a name="authen-apipermissions"></a>

 When you are setting up [access control](authen-toplevel.md#authorization) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The first column in the table lists each ACM API operation\. You specify actions in a policy's `Action` element\. The remaining columns provide the additional information: 

 You can use the IAM policy elements in your ACM policies to express conditions\. For a complete list, see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 

**Note**  
 To specify an action, use the `acm:` prefix followed by the API operation name \(for example, `acm:RequestCertificate`\)\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**ACM API Operations and Permissions**  

| ACM API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [AddTagsToCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html)  |  `acm:AddTagsToCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [DeleteCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html)  |  `acm:DeleteCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [DescribeCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html)  |  `acm:DescribeCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [GetCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html)  |  `acm:GetCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [ImportCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html)  |  `acm:ImportCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [ListCertificates](http://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html)  |  `acm:ListCertificates`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [ListTagsForCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html)  |  `acm:ListTagsForCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [RemoveTagsFromCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html)  |  `acm:RemoveTagsFromCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [RequestCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html)  |  `acm:RequestCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 
|  [ResendValidationEmail](http://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html)  |  `acm:ResendValidationEmail`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_ID`  | 