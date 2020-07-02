# Troubleshoot Certificate Import Problems<a name="troubleshoot-import"></a>

You can import third\-party certificates into ACM and associate them with [integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html.html)\. If you encounter problems, review the [prerequisites](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-prerequisites.html) and [certificate format](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-format.html) topics\. In particular, note the following: 
+ You can import only X\.509 version 3 SSL/TLS certificates\. 
+ Your certificate can be self–signed or it can be signed by a certificate authority \(CA\)\. 
+ If your certificate is signed by a CA, you must include an intermediate certificate chain that provides a path to the root of authority\. 
+ If your certificate is self\-signed, you may need to include an intermediate certificate chain, and you must include the secret key\.
+ Each certificate in the chain must directly certify the one preceding\. 
+ Do not include your end\-entity certificate in the intermediate certificate chain\.
+ Your certificate, certificate chain, and private key \(if any\) must be PEM–encoded\. 
+ Your private key \(if any\) must not be encrypted\. 
+ Services [integrated](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html) with ACM must use ACM\-supported algorithms and key sizes\. See the AWS Certificate Manager User Guide and the documentation for each service to make sure that your certificate will work\. 
+ Certificate support by integrated services might differ depending on whether the certificate is imported into IAM or into ACM\. 
+ The certificate must be valid when it is imported\. 
+ Detail information for all of your certificates is displayed in the console\. By default, however, if you call the [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html) API or the [list\-certificates](https://docs.aws.amazon.com/cli/latest/reference/acm/list-certificates.html) AWS CLI command without specifying the `keyTypes` filter, only `RSA_1024` or `RSA_2048` certificates are displayed\. 