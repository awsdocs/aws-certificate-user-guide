# Key Attribute Reference<a name="key-attribute-table"></a>

The key\_mgmt\_util commands use constants to represent the attributes of keys in an HSM\. This topic can help you to identify the attributes, find the constants that represent them in commands, and understand their values\. 

You set the attributes of a key when you create it\. To change the token attribute, which indicates whether a key is persistent or exists only in the session, use the [setAttribute](key_mgmt_util-setAttribute.md) command in key\_mgmt\_util\. To change the label, wrap, unwrap, encrypt, or decrypt attributes, use the `setAttribute` command in cloudhsm\_mgmt\_util\.

To get a list of attributes and their constants, use [listAttributes](key_mgmt_util-listAttributes.md)\. To get the attribute values for a key, use [getAttribute](key_mgmt_util-getAttribute.md)\.

The following table lists the key attributes, their constants, and their valid values\.


| Attribute | Constant | Values | 
| --- | --- | --- | 
|  OBJ\_ATTR\_CLASS  |  0  | **2**: Public key in a public–private key pair\.3: Private key in a public–private key pair\.**4**: Secret \(symmetric\) key\. | 
|  OBJ\_ATTR\_TOKEN  |  1  |  **0**: False\. Session key\. **1**: True\. Persistent key\.  | 
|  OBJ\_ATTR\_PRIVATE  |  2  |  **0**: False\.  **1**: True\. Private key in a public–private key pair\.  | 
|  OBJ\_ATTR\_LABEL  |  3  | User\-defined string\. It does not have to be unique in the cluster\. | 
|  OBJ\_ATTR\_KEY\_TYPE  | 256 |  **0**: RSA\.**1**: DSA\.**3**: EC\. **16**: Generic secret\. **18**: RC4\. **21**: Triple DES \(3DES\)\. **31**: AES\. | 
|  OBJ\_ATTR\_ID  | 258 |  User\-defined string\. Must be unique in the cluster\. The default is an empty string\. | 
|  OBJ\_ATTR\_SENSITIVE  |  259  |  **0**: False\. Public key in a public–private key pair\. **1**: True\.   | 
|  OBJ\_ATTR\_ENCRYPT  |  260  |  **0**: False\.  **1**: True\. The key can be used to encrypt data\.   | 
|  OBJ\_ATTR\_DECRYPT  |  261  |  **0**: False\.  **1**: True\. The key can be used to decrypt data\.  | 
|  OBJ\_ATTR\_WRAP  |  262  |  **0**: False\.  **1**: True\. The key can be used to encrypt keys\.  | 
|  OBJ\_ATTR\_UNWRAP  |  263  |  **0**: False\.  **1**: True\. The key can be used to decrypt keys\.  | 
|  OBJ\_ATTR\_SIGN  |  264  |  **0**: False\.  **1**: True\. The key can be used for signing \(private keys\)\.  | 
|  OBJ\_ATTR\_VERIFY  |  266  |  **0**: False\.  **1**: True\. The key can be used for verification \(public keys\)\.  | 
|  OBJ\_ATTR\_MODULUS  |  288  |  The modulus that was used to generate an RSA key pair\.  For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_MODULUS\_BITS  |  289  |  The length of the modulus used to generate an RSA key pair\. For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_PUBLIC\_EXPONENT  |  290  |  The public exponent used to generate an RSA key pair\. For other key types, this attribute does not exist\.  | 
|  OBJ\_ATTR\_VALUE\_LEN  |  353  |  Key length in bits\.  | 
|  OBJ\_ATTR\_EXTRACTABLE  |  354  |  **0**: False\.  **1**: True\. The key can be exported from the HSMs\.  | 
|  OBJ\_ATTR\_LOCAL  |  355  |  **0**\. False\. The key was imported into the HSMs\. **1**\. True\.   | 
|  OBJ\_ATTR\_KCV  |  371  |  Key check value of the key\. For more information, see [Additional Details](#key-attribute-table-details)\.  | 
|  OBJ\_ATTR\_ALL  |  512  |  Represents all attributes\.  | 

## Additional Details<a name="key-attribute-table-details"></a>

**Key check value \(kcv\)**  
The *key check value* \(KCV\) is an 8\-byte hash or checksum of a key\. The HSM calculates a KCV when it generates the key\. You can also calculate a KCV outside of the HSM, such as after you export a key\. You can then compare the KCV values to confirm the identity and integrity of the key\. To get the KCV of a key, use [getAttribute](key_mgmt_util-getAttribute.md)\.  
AWS CloudHSM uses the following standard method to generate a key check value:  

+ **Symmetric keys**: First 8 bytes of the result of encrypting 16 zero\-filled bytes with the key\.

+ **Asymmetric key pairs**: First 8 bytes of the modulus hash\.