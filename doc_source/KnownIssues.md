# Known Issues<a name="KnownIssues"></a>

The following issues are currently known for AWS CloudHSM\.


+ [Known Issues for all HSM instances](#ki-all)
+ [Known Issues for the PKCS\#11 SDK](#ki-pkcs11-sdk)
+ [Known Issues for the JCE SDK](#ki-jce-sdk)
+ [Known Issues for the OpenSSL SDK](#ki-openssl-sdk)

## Known Issues for all HSM instances<a name="ki-all"></a>

The following issues impact all AWS CloudHSM users regardless of whether they use the key\_mgmt\_util command line tool, the PKCS\#11 SDK, the JCE SDK, or the OpenSSL SDK\. 

**Issue: **AES key wrapping uses PKCS\#5 padding instead of providing a standards\-compliant implementation of key wrap with zero padding\. Additionally, key wrap with no padding is not supported\.

+ **Impact: **There is no impact if you wrap and unwrap within AWS CloudHSM\. Keys wrapped with AWS CloudHSM cannot be unwrapped within other HSMs or software which expect compliance to the no padding specification\. This is because you may see up to 16 bytes of padding data suffixed to your key data following a standards\-compliant unwrap\. Externally wrapped keys cannot be properly unwrapped into a AWS CloudHSM instance\. 

+ **Workaround: **If you are attempting to externally unwrap a key that was wrapped with AES Key Wrapping on a AWS CloudHSM instance, you must strip the extra padding before attempting to use the key\. You can do this by trimming the extra bytes in a file editor or copying only the key bytes into a new buffer in your code\. 

+ **Resolution Status: **We are fixing the client and SDKs to provide SP800\-38F compliant AES key wrapping\. Updates will be announced to the AWS CloudHSM forum and to the version history page\. The update will include mechanisms to assist you in re\-wrapping any existing wrapped keys in a standards\-compliant way\. 

**Issue: **Imported keys cannot be specified as non\-exportable\.

+ **Impact: **When importing a key through PKCS\#11 or JCE, you are not able to specify that it be made non\-exportable\. In the PKCS\#11 context, the CKA\_NON\_EXPORTABLE flag is ignored\. Additionally, AWS CloudHSM does not support changing the exportable attribute for an existing key\. Because you can neither set a key as non\-exportable during import nor change its exportable attribute after the fact, users today are unable to set imported keys as non\-exportable\. 

+ **Workaround: **You can partially enforce similar behavior by creating a special Cryptographic User \(CU\) for this key, and using the 'share' functionality to allow other users/applications to use the key\. The limitations placed on shared keys prevent other users from exporting the key\. The cryptographic user \(CU\) who imports the key owns the key\. Only this CU can export the key\. You can share the imported key with other CU accounts\. These CU accounts can use the key, but cannot export it, delete it, or share it with other CUs\. 

+ **Resolution Status: **We are implementing fixes to correctly set an imported key to non\- exportable when so specified\. No action will be required on your part to benefit from the fix\.

**Issue: **The client daemon requires at least one valid IP address in its configuration file to successfully connect to the cluster\. 

+ **Impact: **If you delete every HSM in your cluster and then add another HSM which gets a new IP address, the client daemon continues to search for your HSMs at their original IP addresses\. 

+ **Workaround: **If you run an intermittent workload, we recommend that you use the `IpAddress` argument in the [CreateHsm](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) function to set the elastic network interface \(ENI\) to its original value\. Note than an ENI is specific to an availability zone \(AZ\)\. The alternative is to delete the `/opt/cloudhsm/daemon/1/cluster.info` file and then reset the client configuration to the IP address of your new HSM\. You can use the `client -a <IP address>` command\. For more information, see [ Edit the Client Configuration ](http://docs.aws.amazon.com/cloudhsm/latest/userguide/nstall-and-configure-client.html#edit-client-configuration)\. 

**Issue: **There is an upper limit of 16KB on data that can be hashed and signed by AWS CloudHSM\. 

+ **Impact: ** In general, you cannot use the AWS CloudHSM SDKs to hash or sign data longer than 16KB\. It doesn't matter whether you send the data in one piece or in multiple pieces\. The specific impact depends on the SDK you are using\. See the individual SDK sections below for more information\. 

+ **Workaround: **If your data buffer is larger than 16KB, hash your data within your application and use AWS CloudHSM only for signing\. 

+ **Resolution Status: **We are fixing the client and the SDKs to explicitly fail if the data buffer is larger than the 16KB\. Updates will be announced to the AWS CloudHSM forum and to the version history page\.

## Known Issues for the PKCS\#11 SDK<a name="ki-pkcs11-sdk"></a>

**Issue: **PKCS\#11\-compliant error messages are not directly available from the HSM instance\.

+ **Impact: **The PKCS\#11 library makes two round trips to the HSM per call – first to verify whether the requested operation is permitted, and second to execute the operation if it is permitted\. This results in slower performance\. 

+ **Workaround: **We have an alternative implementation of the PKCS\#11 library which relies on a local REDIS cache so that correctness of the requested operation can be checked locally\. There are pros and cons to using this workaround\. 

+ **Resolution status: **We are implementing fixes to directly provide PKCS\#11\-compliant error messages, removing the need for the REDIS workaround\. The updated PKCS\#11 library will be announced to the version history page\. 

**Issue: **The CKA\_DERIVE attribute is not supported and is not handled\.

+ **Impact: **Specifying the CKA\_DERIVE attribute for a key, whether the attribute is set to true or false, will cause the key operation to fail with an invalid template error\. 

+ **Workaround: **Do not specify the CKA\_DERIVE attribute in your code\.

+ **Resolution status: **We are implementing fixes to accept CKA\_DERIVE if it is set to FALSE\. CKA\_DERIVE set to true will not be supported until we begin to add key derivation function support to AWS CloudHSM\. The updated PKCS\#11 library will be announced to the version history page\.

**Issue:** The CKA\_SENSITIVE attribute is not supported and is not handled\.

+ **Impact: **Specifying the CKA\_SENSITIVE attribute for a key, whether the attribute is set to true or false, will cause the key operation to fail with an invalid template error\. 

+ **Workaround: **Do not specify the CKA\_SENSITIVE attribute in your code\.

+ **Resolution status: **We are implementing fixes to accept and properly honor the CKA\_SENSITIVE attribute\. The updated PKCS\#11 library will be announced to the version history page\. 

**Issue: **Multi\-part hashing and signing are not supported\.

+ **Impact: **`C_DigestUpdate` and `C_DigestFinal` are not implemented\. `C_SignFinal` is also not implemented and will fail with CKR\_ARGUMENTS\_BAD for a non\-NULL buffer\. 

+ **Workaround: **Hash your data within your application and use AWS CloudHSM only for signing the hash\. 

+ **Resolution status: **We are fixing the client and the SDKs to correctly implement multi\-part hashing\. Updates will be announced to the AWS CloudHSM forum and to the version history page\. 

**Issue: **`C_GenerateKeyPair` does not handle `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT` in the private template in a manner that is compliant with standards\.

+ **Impact: `C_GenerateKeyPair` should return `CKA_TEMPLATE_INCONSISTENT` when the private template contains `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT`\. It instead generates a private key for which all usage fields are set to `FALSE`\. The key cannot be used\.** 

+ **Workaround: **We recommend that your application check the usage field values in addition to the error code\.

+ **Resolution status: **We are implementing fixes to return the proper error message when an incorrect private key template is used\. The updated PKCS\#11 library will be announced in the version history page\. 

**Issue: **You cannot hash more than 16KB of data\. For larger buffers, only the first 16KB will be hashed and returned\. The excess data will be silently ignored\.

+ **Impact: **C\_DIGEST will return an incorrect result if the data size exceeds 16KB\. 

+ **Workaround: **You should hash your data within your application and use AWS CloudHSM to sign the hash\.

+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We are evaluating alternatives to support hashing larger buffers without relying on multi\-part hashing\. Updates will be announced to the AWS CloudHSM forum and to the version history page\. 

**Issue: **Buffers for the `C_Encrypt`and `C_Decrypt` APIs cannot exceed 16KB when using the `CKM_AES_GCM` mechanism\. Also, AWS CloudHSM does not support multi\-part AES\-GCM encryption\.

+ **Impact: **You cannot use the `CKM_AES_GCM` mechanism to encrypt data larger than 16KB\.

+ **Workaround: ** You can use an alternative mechanism such as `CKM_AES_CBC` or you can divide your data into pieces and encrypt each piece individually\. You must manage the division of your data and subsequent encryption\. AWS CloudHSM does not perform multi\-part AES\-GCM encryption for you\. Note that FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM\. Therefore, the IV for each piece of your AES\-GCM encrypted data will be different\. 

+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We are evaluating alternatives to support larger buffers without relying on multi\-part encryption\. Updates will be announced to the AWS CloudHSM forum and to the version history page\. 

## Known Issues for the JCE SDK<a name="ki-jce-sdk"></a>

**Issue: **Key wrap and key unwrap functions are not implemented\.

+ **Impact: **You cannot programmatically wrap or unwrap keys using the JCE\.

+ **Workaround: **You can script key\_mgmt\_util to wrap and unwrap keys\.

+ **Resolution status: **We are planning to add support for key wrap and unwrap directly through the JCE\. The update will be announced on the version history page once available\. 

**Issue: **If the client daemon is restarted, the JCE application must be stopped and restarted as well\. 

+ **Impact: **Following a restart of the client daemon for any reason, your Java application will not successfully communicate with your HSM instances until the application is restarted\.

+ **Workaround: **You should continuously monitor the client daemon for execution status\. If the daemon needs to be restarted for any reason, you should stop and start the application as well\.

+ **Resolution status: **We are fixing the SDK and client so that they will handle a restart of the client daemon without having to restart the application\. The update will be announced on the version history page once available\.

**Issue: **The JCE KeyStore is read\-only\.

+ **Impact: **You cannot store an object type that is not supported by the HSM in the JCE Keystore today\. Specifically, you cannot store certificates in the keystore\. This precludes interoperability with tools such as jarsigner which expect to find the certificate in the keystore\. 

+ **Workaround: **You can rework your code to load certificates from local files or from an S3 bucket location instead of from the keystore\. 

+ **Resolution status: **We are adding support for certificate storage in the keystore\. The feature will be announced on the version history page once available\.

**Issue: **

+ **Impact: **

+ **Workaround: **

+ **Resolution status: **

**Issue: **Buffers for AES\-GCM encryption cannot exceed 16KB\. Also, multi\-part AES\-GCM is not supported\.

+ **Impact: **You cannot use the AES\-GCM to encrypt data larger than 16KB\.

+ **Workaround: ** You can use an alternative mechanism such as AES\-CBC or you can divide your data into pieces and encrypt each piece individually\. You must manage the division of your data and subsequent encryption\. AWS CloudHSM does not perform multi\-part AES\-GCM encryption for you\. Note that FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM\. Therefore, the IV for each of your AES\-GCM encrypted pieces of data will be different\. 

+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We are evaluating alternatives to support larger buffers without relying on multi\-part encryption\. Updates will be announced to the AWS CloudHSM forum and to the version history page\. 

## Known Issues for the OpenSSL SDK<a name="ki-openssl-sdk"></a>

**Issue:** Only RSA offload to the HSM is supported by default\.

+ **Impact: **To maximize performance, the SDK is not configured to offload additional functions such as random number generation or EC\-DH operations\.

+ **Workaround: **Please contact us through a support case if you need to offload additional operations\.

+ **Resolution status: **We are adding support to the SDK to configure offload options through a configuration file\. The update will be announced on the version history page once available\.

**Issue: **If the client daemon is restarted, any application using the OpenSSL SDK \(such as NGINX\) must be stopped and restarted as well\.

+ **Impact: **Following a restart of the client daemon for any reason, your application will not successfully communicate with your HSM instances until the application is restarted\. 

+ **Workaround: **You should continuously monitor the client daemon for execution status\. If the daemon needs to be restarted for any reason, you should stop and start the application as well\. 

+ **Resolution status: **We are fixing the SDK so that you do not need to restart the application even if the client daemon is restarted\. The update will be announced on the version history page once available\. 

**Issue:** RSA encryption and decryption with OAEP padding using a key on the HSM is not supported\.

+ **Impact: **Any call to RSA encryption and decryption with OAEP padding fails with a divide by zero error\. This occurs because the OpenSSL library calls the operation locally using the fake PEM file instead of offloading the operation to the HSM\. 

+ **Workaround: ** You can perform this procedure by using either the [AWS CloudHSM Software Library for PKCS \#11](pkcs11-library.md) or the [AWS CloudHSM Software Library for Java](java-library.md)\. 

+ **Resolution status: **We are adding support to the SDK to correctly offload this operation\. The update will be announced on the version history page once available\. 