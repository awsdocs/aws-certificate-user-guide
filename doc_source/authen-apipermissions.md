# ACM API Permissions: Actions and Resources Reference<a name="authen-apipermissions"></a>

When you set up [access control](security-iam.md#authorization) and write permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The first column in the table lists each AWS Certificate Manager API operation\. You specify actions in a policy's `Action` element\. The remaining columns provide the additional information: 

 You can use the IAM policy elements in your ACM policies to express conditions\. For a complete list, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 

**Note**  
 To specify an action, use the `acm:` prefix followed by the API operation name \(for example, `acm:RequestCertificate`\)\. 

Use the scroll bars to see the rest of the table\.


**ACM API Operations and Permissions**  

| ACM API Operations | Required Permissions \(API Operations\) | Resources | 
| --- | --- | --- | 
|  [AddTagsToCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html)  |  `acm:AddTagsToCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [DeleteCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html)  |  `acm:DeleteCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html)  |  `acm:DescribeCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [ExportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ExportCertificate.html)  |  `acm:ExportCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [GetCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html)  |  `acm:GetCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html)  |  `acm:ImportCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html)  |  `acm:ListCertificates`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [ListTagsForCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html)  |  `acm:ListTagsForCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [RemoveTagsFromCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html)  |  `acm:RemoveTagsFromCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html)  |  `acm:RequestCertificate`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 
|  [ResendValidationEmail](https://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html)  |  `acm:ResendValidationEmail`  |  `arn:aws:acm:AWS_region:AWS_account_ID:certificate/certificate_authority_ID`  | 