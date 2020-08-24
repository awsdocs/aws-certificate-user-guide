# Using a Service Linked Role \(SLR\) with ACM<a name="acm-slr"></a>

AWS Certificate Manager uses an AWS Identity and Access Management \(IAM\)[ service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) to enable automatic renewals of managed ACM certificates\. A service\-linked role \(SLR\) is an IAM role that is linked directly to the ACM service\. SLRs are predefined by ACM and include all the permissions that the service requires to call other AWS services on your behalf\.

The SLR makes setting up ACM easier because you don’t have to manually add the necessary permissions for unattended certificate signing\. ACM defines the permissions of its SLR, and unless defined otherwise, only ACM can assume the role\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support SLRs, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the SLR documentation for that service\.

## SLR Permissions for ACM<a name="slr-permissions"></a>

ACM uses an SLR named Amazon Certificate Manager Service Role Policy\.

The AWSServiceRoleForCertificateManager SLR trusts the following services to assume the role:
+ `acm.amazonaws.com`

The role permissions policy allows ACM to complete the following actions on the specified resources:
+ Actions: `acm-pca:IssueCertificate`, `acm-pca:GetCertificate` on "\*"

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete an SLR\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

**Important**  
ACM may alert you that it cannot determine whether an SLR exists on your account\. If the required `iam:GetRole` permission has already been granted to the ACM SLR for your account, then the alert will not recur after the SLR is created\. If it does recur, then you or your account administrator may need to grant the `iam:GetRole` permission to ACM, or associate your account with the ACM managed policy `AWSCertificateManagerFullAccess`\.

## Creating the SLR for ACM<a name="create-slr"></a>

You don't need to manually create the SLR that ACM uses\. When you issue an ACM certificate using the AWS Management Console, the AWS CLI, or the AWS API, ACM creates the SLR for you the first time that you choose a private CA to sign your certificate\.

If you encounter messages stating that ACM cannot determine whether an SLR exists on your account, it may mean that your account has not granted a read permission that ACM Private CA requires\. This will not prevent the SLR from being installed, and you can still issue certificates, but ACM will be unable to renew the certificates automatically until you resolve the problem\. For more information, see [Problems with the ACM Service\-Linked Role \(SLR\)](slr-problems.md)\.

**Important**  
This SLR can appear in your account if you completed an action in another service that uses the features supported by this role\. Also, if you were using the ACM service before January 1, 2017, when it began supporting SLRs, then ACM created the AWSServiceRoleForCertificateManager role in your account\. To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

If you delete this SLR, and then need to create it again, you can use either of these methods:
+ In the IAM console, choose **Role**, **Create role**, **Certificate Manager** to create a new role with the **CertificateManagerServiceRolePolicy** use case\. 
+ Using the IAM API [CreateServiceLinkedRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateServiceLinkedRole.html) or the corresponding AWS CLI command [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html), create an SLR with the `acm.amazonaws.com` service name\.

 For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

## Editing the SLR for ACM<a name="edit-slr"></a>

ACM does not allow you to edit the AWSServiceRoleForCertificateManager service\-linked role\. After you create an SLR, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the SLR for ACM<a name="delete-slr"></a>

You typically don't need to delete the AWSServiceRoleForCertificateManager SLR\. However, you can delete the role manually using the IAM console, the AWS CLI or the AWS API\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for ACM SLRs<a name="slr-regions"></a>

ACM supports using SLRs in all of the regions where both ACM and ACM Private CA are available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.


****  

| Region name | Region identity | Support in ACM | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Osaka\-Local\) | ap\-northeast\-3 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| AWS GovCloud \(US\-West\) | us\-gov\-west\-1 | Yes | 
| AWS GovCloud \(US\-East\) East | us\-gov\-east\-1 | Yes | 