# genECCKeyPair<a name="key_mgmt_util-genECCKeyPair"></a>

The `genECCKeyPair` command in the key\_mgmt\_util tool generates an [Elliptic Curve Cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) \(ECC\) key pair in your HSMs\. You must specify the elliptic curve type and a label for the keys\. You can also share the private key with other CU users, create non\-extractable keys, quorum\-controlled keys, and keys that expire when the session ends\. When the command succeeds, it returns the key handles that the HSM assigns to the public and private ECC keys\. You can use the key handles to identify the keys to other commands\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

**Tip**  
To find the attributes of a key that you have created, such as the type, length, label, and ID, use [getAttribute](key_mgmt_util-getAttribute.md)\. To find the keys for a particular user, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\. To find keys based on their attribute values, use [findKey](key_mgmt_util-findKey.md)\. 

## Syntax<a name="genECCKeyPair-syntax"></a>

```
genECCKeyPair -h

genECCKeyPair -i <EC curve id> 
              -l <label> 
              [-id <key ID>]
              [-min_srv <minimum number of servers>]
              [-m_value <0..8>]
              [-nex]
              [-sess]
              [-timeout <number of seconds> ]
              [-u <user-ids>]
              [-attest]
```

## Examples<a name="genECCKeyPair-examples"></a>

These examples show how to use genECCKeyPair to create ECC key pairs in your HSMs\.

**Example : Create and Examine an ECC Key Pair**  
This command creates an ECC key pair using an NID\_sect571r1 elliptic curve and an `ecc12` label\. The output shows that the key handle of the private key is `262177` and the key handle of the public key is `262179`\. The label applies to both the public and private keys\.  

```
Command: genECCKeyPair -i 12 -l ecc12

        Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 262179    private key handle: 262177

        Cluster Error Status
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
After generating the key, you might want to examine its attributes\. The next command uses [getAttribute](key_mgmt_util-getAttribute.md) to write all of the attributes \(represented by the constant `512`\) of the new ECC private key to the `attr_262177` file\.  

```
Command: getAttribute -o 262177 -a 512 -out attr_262177
got all attributes of size 529 attr cnt 19
Attributes dumped into attr_262177

        Cfm3GetAttribute returned: 0x00 : HSM Return: SUCCESS
```
This command gets the content of the `attr_262177` attribute file\. The output shows that the key is an elliptic curve private key that can be used for signing, but not for encrypting, decrypting, wrapping, unwrapping, or verifying\. The key is persistent and exportable\.  

```
$  cat attr_262177

OBJ_ATTR_CLASS
0x03
OBJ_ATTR_KEY_TYPE
0x03
OBJ_ATTR_TOKEN
0x01
OBJ_ATTR_PRIVATE
0x01
OBJ_ATTR_ENCRYPT
0x00
OBJ_ATTR_DECRYPT
0x00
OBJ_ATTR_WRAP
0x00
OBJ_ATTR_UNWRAP
0x00
OBJ_ATTR_SIGN
0x01
OBJ_ATTR_VERIFY
0x00
OBJ_ATTR_LOCAL
0x01
OBJ_ATTR_SENSITIVE
0x01
OBJ_ATTR_EXTRACTABLE
0x01
OBJ_ATTR_LABEL
ecc2
OBJ_ATTR_ID

OBJ_ATTR_VALUE_LEN
0x0000008a
OBJ_ATTR_KCV
0xbbb32a
OBJ_ATTR_MODULUS
044a0f9d01d10f7437d9fa20995f0cc742552e5ba16d3d7e9a65a33e20ad3e569e68eb62477a9960a87911e6121d112b698e469a0329a665eba74ee5ac55eae9f5
OBJ_ATTR_MODULUS_BITS
0x0000019f
```

**Example Using an Invalid EEC Curve**  
This command attempts to create an ECC key pair by using an NID\_X9\_62\_prime192v1 curve\. Because this elliptic curve is not valid for FIPS\-mode HSMs, the command fails\. The message reports that a server in the cluster is unavailable, but this does not typically indicate a problem with the HSMs in the cluster\.  

```
Command:  genECCKeyPair -i 1 -l ecc1

        Cfm3GenerateKeyPair returned: 0xb3 : HSM Error: This operation violates the current configured/FIPS policies

        Cluster Error Status
        Node id 0 and err state 0x30000085 : HSM CLUSTER ERROR: Server in cluster is unavailable
```

## Parameters<a name="genECCKeyPair-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-i **  
Specifies the identifier for the elliptic curve\. Enter an identifier \(1 \- 15\)\.   
Valid values:   

+ **1**: NID\_X9\_62\_prime192v1

+ **2**: NID\_X9\_62\_prime256v1

+ **3**: NID\_sect163k1

+ **4**: NID\_sect163r2

+ **5**: NID\_sect233k1

+ **6**: NID\_sect233r1

+ **7**: NID\_sect283k1

+ **8**: NID\_sect283r1

+ **9**: NID\_sect409k1

+ **10**: NID\_sect409r1

+ **11**:NID\_sect571k1

+ **12**: NID\_sect571r1

+ **13**: NID\_secp224r1

+ **14**:NID\_secp384r1

+ **15**: NID\_secp521r1
Required: Yes

**\-l**  
Specifies a user\-defined label for the key pair\. Type a string\. The same label applies to both keys in the pair  
You can use any phrase that helps you to identify the key\. Because the label does not have to be unique, you can use it to group and categorize keys\.   
Required: Yes

**\-id**  
Specifies a user\-defined identifier for the key pair\. Type a string that is unique in the cluster\. The default is an empty string\. The ID that you specify applies to both keys in the pair\.  
Default: No ID value\.  
Required: No

**\-min\_srv**  
Specifies the minimum number of HSMs on which the key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**\-m\_value**  
Specifies the number of users who must approve any cryptographic operation that uses the private key in the pair\. Type a value from `0` to `8`\.  
This parameter establishes a quorum authentication requirement for the private key\. The default value, `0`, disables the quorum authentication feature for the key\. When quorum authentication is enabled, the specified number of users must sign a token to approve cryptographic operations that use the private key, and operations that share or unshare the private key\.  
To find the `m_value` of a key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\.  
This parameter is valid only when the `-u` parameter in the command shares the key pair with enough users to satisfy the `m_value` requirement\.  
Default: 0  
Required: No

**\-nex**  
Makes the private key nonextractable\. The private key that is generated cannot be [exported from the HSM](manage-keys.md#export-keys)\. Public keys are always extractable\.  
Default: Both the public and private keys in the key pair are extractable\.  
Required: No

**\-sess**  
Creates a key that exists only in the current session\. The key cannot be recovered after the session ends\.  
Use this parameter when you need a key only briefly, such as a wrapping key that encrypts, and then quickly decrypts, another key\. Do not use a session key to encrypt data that you might need to decrypt after the session ends\.  
To change a session key to a persistent \(token\) key, use [setAttribute](key_mgmt_util-setAttribute.md)\.  
Default: The key is persistent\.   
Required: No

**\-timeout**  
Specifies how long \(in seconds\) the command waits for a key to be synchronized to the number of HSMs specified by the `min_srv` parameter\.   
This parameter is valid only when the `min_srv` parameter is also used in the command\.  
Default: No timeout\. The command waits indefinitely and returns only when the key is synchronized to the minimum number of servers\.  
Required: No

**\-u**  
Shares the private key in the pair with the specified users\. This parameter gives other HSM crypto users \(CUs\) permission to use the private key in cryptographic operations\. Public keys can be used by any user without sharing\.  
Type a comma\-separated list of HSM user IDs, such as \-`u 5,6`\. Do not include the HSM user ID of the current user\. To find HSM user IDs of CUs on the HSM, use [listUsers](key_mgmt_util-listUsers.md)\. To share and unshare existing keys, use shareKey\.   
Default: Only the current user can use the private key\.   
Required: No

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered with\.  
Default: No attestation check\.  
Required: No

## Related Topics<a name="genECCKeyPair-seealso"></a>

+ [genSymKey](key_mgmt_util-genSymKey.md)

+ [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md)

+ [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md)