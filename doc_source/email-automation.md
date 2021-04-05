# Automating email validation<a name="email-automation"></a>

ACM certificates that were email\-validated normally require manual intervention by the domain owner\. Organizations dealing with large numbers of email\-validated certificates may prefer to create a parser that can automate the required responses\. To assist customers using email validation, the information in this section describes the template used for domain validation email messages and the workflow involved in completing the validation process\. 

## The validation email template<a name="validation-email-template"></a>

Validation email messages have the following format\. The content of the highlighted strings should be replaced with values that are specific to the domain being validated\.

```
Greetings from Amazon Web Services,

We received a request to issue an SSL/TLS certificate for $requested_domain.

Verify that the following domain, AWS account ID, and certificate identifier correspond 
to a request from you or someone in your organization.

Domain: $fqdn
AWS account ID: $account_id
AWS Region name: $region_name
Certificate Identifier: $certificate_identifier

To approve this request, go to Amazon Certificate Approvals 
(https://$region_name.acm-certificates.amazon.com/approvals?code=$validation_code&context=$validation_context) 
and follow the instructions on the page.

This email is intended solely for authorized individuals for $fqdn. To express any concerns
about this email or if this email has reached you in error, forward it along with a brief 
explanation of your concern to validation-questions@amazon.com.

Sincerely,
Amazon Web Services
```

**Note**  
Once you receive a new validation message from AWS, we recommend that you use it as the most up\-to\-date and authoritative template for your parser\.

Customers with message parsers designed before November, 2020, should note the following changes that may have been made to the template\.
+ The email subject line now reads "Certificate request for *domain name*" instead of "Certificate approval for *domain name*"\.
+ The **AWS account ID** is now presented without dashes or hyphens\. 
+ The **Certificate Identifier** now presents the entire certificate ARN instead of a shortened form, for example, *arn:aws:acm:us\-east\-1:000000000000:certificate/3b4d78e1\-0882\-4f51\-954a\-298ee44ff369* rather than *3b4d78e1\-0882\-4f51\-954a\-298ee44ff369*\.
+ The certificate approval URL now contains **acm\-certificates\.amazon\.com** instead of **certificates\.amazon\.com**\.
+ The approval form opened by clicking the certificate approval URL now contains the approval button\. The name of the approval button div is now **approve\-button** instead of **approval\_button**\.
+ Validation messages for both newly requested certificates and renewing certificates have the same email format\.

## Validation workflow<a name="validation-workflow"></a>

This section provides information about the renewal workflow for email\-validated certificates\. 
+ When the ACM console processes a multi\-domain certificate request, it sends validation email messages to the first domain it finds that includes an MX record\. The domain owner needs to validate an email message for each domain before ACM can issue the certificate\. For more information, see [Using Email to Validate Domain Ownership](https://docs.aws.amazon.com/acm/latest/userguide/email-validation.html)\. 
+ Email validation for multi\-domain certificate requests using the ACM API or CLI results in an email message being sent by default to the apex domain and to each subdomain\. The domain owner needs to validate an email message for each of these domains before ACM can issue the certificate\.
**Note**  
Prior to November, 2020, customers needed to validate only the apex domain and ACM would issue a certificate that also covered any subdomains\. Customers with message parsers designed before that time should note the change to the email validation workflow\.
+ With the ACM API or CLI, you can force all validation email messages for a multi\-domain certificate request to be sent to the apex domain\. In the API, use the `DomainValidationOptions` parameter of the [https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) action to specify a value for `ValidationDomain`, which is a member of the [https://docs.aws.amazon.com/acm/latest/APIReference/API_DomainValidationOption.html](https://docs.aws.amazon.com/acm/latest/APIReference/API_DomainValidationOption.html) type\. In the CLI, use the \-\-domain\-validation\-options parameter of the [https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/acm/request-certificate.html) command to specify a value for ValidationDomain\.