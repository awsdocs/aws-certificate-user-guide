# Troubleshoot Certificate Importing Problems<a name="troubleshoot-import"></a>

You can import third party certificates into ACM and associate them with [integrated services](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html.html)\. If you encounter problems, review the [prerequisites](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-prerequisites.html) and [certificate format](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-format.html) topics\. In particular, note the following: 
+  You can import only X\.509 version 3 SSL/TLS certificates\. 
+  Your certificate can be self–signed or it can be signed by a certificate authority \(CA\)\. 
+  If your certificate is signed by a CA, you must include a certificate chain that chains up to the root of authority\. 
+  Do not include your certificate in the certificate chain\. 
+  Each certificate in the chain must directly certify the one preceding\. 
+  Your certificate, private key, and certificate chain must be PEM–encoded\. 
+  Your private key must not be encrypted\. 
+  Services [integrated](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html) with ACM allow only algorithms and key sizes they support to be associated with their resources\. Support can change\. See the documentation for each service to make sure your certificate will work\. 
+ Certificate support by integrated services might differ depending on whether the certificate is imported into IAM or into ACM\. 
+  The certificate must be valid when it is imported\. 
+  Detail information for all of your certificates is displayed in the console\. By default, however, if you call the [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html) API or the [list\-certificates](https://docs.aws.amazon.com/cli/latest/reference/acm/list-certificates.html) AWS CLI command without specifying the `keyTypes` filter, only `RSA_1024` or `RSA_2048` certificates are displayed\. 