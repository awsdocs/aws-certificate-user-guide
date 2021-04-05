# Installing ACM certificates<a name="gs-acm-install"></a>

You cannot use ACM to install a public certificate directly on your AWS based website or application\. You must use one of the services integrated with ACM\. For more information, see [Services integrated with AWS Certificate Manager](acm-services.md)\. 

ACM certificates signed by a CA in ACM Private CA and intended for your private PKI can be [exported](https://docs.aws.amazon.com/acm/latest/userguide/export-private.html) and installed manually on any system where you have administrative access\. These certificates are not trusted on the public internet\.