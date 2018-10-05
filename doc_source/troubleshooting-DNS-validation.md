# Troubleshoot DNS Validation Problems<a name="troubleshooting-DNS-validation"></a>

Consult the following topic if you experience DNS validation problems\.

## Underscores Prohibited by DNS Provider<a name="troubleshooting-DNS"></a>

If your DNS provider prohibits leading underscores in CNAME values, you can remove the underscore from the ACM\-provided value and validate your domain without it\. For example, the CNAME value `_x2.acm-validations.aws` can be changed to `x2.acm-validations.aws` for validation purposes\. However, the CNAME name parameter must always begin with a leading underscore\.

You can use either of the values on the right side of the table below to validate a domain\.


| Name | Type | Value | 
| --- | --- | --- | 
| \_<random value>\.example\.com\. | CNAME | \_<random value>\.acm\-validations\.aws\.  | 
| \_<random value>\.example\.com\. | CNAME | <random value>\.acm\-validations\.aws\.  | 