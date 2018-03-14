# genSymKey<a name="key_mgmt_util-genSymKey"></a>

The genSymKey command in the key\_mgmt\_util tool generates a symmetric key in your HSMs\. You can specify the key type and size, assign an ID and label, and share the key with other HSM users\. You can also create nonextractable keys and keys that expire when the session ends\. When the command succeeds, it returns a key handle that the HSM assigns to the key\. You can use the key handle to identify the key to other commands\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

**Tip**  
To find the attributes of a key that you have created, such as the type, length, label, and ID, use [getAttribute](key_mgmt_util-getAttribute.md)\. To find the keys for a particular user, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\. To find keys based on their attribute values, use [findKey](key_mgmt_util-findKey.md)\. 

## Syntax<a name="genSymKey-syntax"></a>

```
genSymKey -h

genSymKey -t <key-type>
          -s <key-size> 
          -l <label> 
          [-id <key-ID>] 
          [-min_srv <minimum-number-of-servers>] 
          [-m_value <0..8>]
          [-nex] 
          [-sess] 
          [-timeout <number-of-seconds> ]
          [-u <user-ids>] 
          [-attest]
```

## Examples<a name="genSymKey-examples"></a>

These examples show how to use genSymKey to create symmetric keys in your HSMs\.

**Example : Generate an AES Key**  
This command creates a 256\-bit AES key with an `aes256` label\. The output shows that the key handle of the new key is `6`\.  

```
Command: genSymKey -t 31 -s 32 -l aes256

        Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Created.  Key Handle: 6

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Create a Session Key**  
This command creates a nonextractable 192\-bit AES key that is valid only in the current session\. You might want to create a key like this to wrap \(and then immediately unwrap\) a key that is being exported\.   

```
Command: genSymKey -t 31 -s 24 -l tmpAES -id wrap01 -nex -sess 
```

**Example : Return Quickly**  
This command creates a generic 512\-byte key with a label of `IT_test_key`\. The command does not wait for the key to be synchronized to all HSMs in the cluster\. Instead, it returns as soon as the key is created on any one HSM \(`-min_srv 1`\) or in 1 second \(`-timeout 1`\), whichever is shorter\. If the key is not synchronized to the specified minimum number of HSMs before the timeout expires, it is not generated\. You might want to use a command like this in a script that creates numerous keys, like the `for` loop in the following example\.   

```
Command: genSymKey -t 16 -s 512 -l IT_test_key -min_srv 1 -timeout 1

$  for i in {1..30}; 
     do /opt/cloudhsm/bin/key_mgmt_util Cfm3Util singlecmd loginHSM -u CU -s example_user -p example_pwd genSymKey -l aes -t 31 -s 32 -min_srv 1 -timeout 1; 
 done;
```

**Example : Create a Quorum Authorized Generic Key**  
This command creates a 2048\-bit generic secret key with the label `generic-mV2`\. The command uses the `-u` parameter to share the key with another CU, user 6\. It uses the `-m_value` parameter to require a quorum of at least two approvals for any cryptographic operations that use the key\. The command also uses the `-attest` parameter to verify the integrity of the firmware on which the key is generated\.  
The output shows that the command generated a key with key handle `9` and that the attestation check on the cluster firmware passed\.  

```
                Command:  genSymKey -t 16 -s 2048 -l generic-mV2 -m_value 2 -u 6 -attest

        Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Created.  Key Handle: 9

        Attestation Check : [PASS]

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Create and Examine a Key**  
This command creates a Triple DES key with a `3DES_shared` label and an ID of `IT-02`\. The key can be used by the current user, and users 4 and 5\. The command fails if the ID is not unique in the cluster or if the current user is user 4 or 5\.   
The output shows that the new key has key handle `7`\.  

```
Command: genSymKey -t 21 -s 24 -l 3DES_shared -id IT-02 -u 4,5

       Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Created.  Key Handle: 7

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
To verify that the new 3DES key is owned by the current user and shared with users 4 and 5, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\. The command uses the handle that was assigned to the new key \(`Key Handle: 7`\)\.  
The output confirms that the key is owned by user 3 and shared with users 4 and 5\.  

```
Command:  getKeyInfo -k 7

        Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

        Owned by user 3

        also, shared to following 2 user(s):

                 4, 5
```
To confirm the other properties of the key, use [getAttribute](key_mgmt_util-getAttribute.md)\. The first command uses `getAttribute` to get all attributes \(`-a 512`\) of key handle 7 \(`-o 7`\)\. It writes them to the `attr_7` file\. The second command uses `cat` to get the contents of the `attr_7` file\.   
This command confirms that key 7 is a 192\-bit \(`OBJ_ATTR_VALUE_LEN 0x00000018` or 24\-byte\) 3DES \(`OBJ_ATTR_KEY_TYPE 0x15`\) symmetric key \(`OBJ_ATTR_CLASS 0x04`\) with a label of `3DES_shared` \(`OBJ_ATTR_LABEL 3DES_shared`\) and an ID of `IT_02` \(`OBJ_ATTR_ID IT-02`\)\. The key is persistent \(`OBJ_ATTR_TOKEN 0x01`\) and extractable \(`OBJ_ATTR_EXTRACTABLE 0x01`\) and can be used for encryption, decryption, and wrapping\.   
For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  

```
Command:  getAttribute -o 7 -a 512 -attr_7

got all attributes of size 444 attr cnt 17
Attributes dumped into attr_7 file

        Cfm3GetAttribute returned: 0x00 : HSM Return: SUCCESS


$  cat attr_7

OBJ_ATTR_CLASS
0x04
OBJ_ATTR_KEY_TYPE
0x15
OBJ_ATTR_TOKEN
0x01
OBJ_ATTR_PRIVATE
0x01
OBJ_ATTR_ENCRYPT
0x01
OBJ_ATTR_DECRYPT
0x01
OBJ_ATTR_WRAP
0x00
OBJ_ATTR_UNWRAP
0x00
OBJ_ATTR_SIGN
0x00
OBJ_ATTR_VERIFY
0x00
OBJ_ATTR_LOCAL
0x01
OBJ_ATTR_SENSITIVE
0x01
OBJ_ATTR_EXTRACTABLE
0x01
OBJ_ATTR_LABEL
3DES_shared
OBJ_ATTR_ID
IT-02
OBJ_ATTR_VALUE_LEN
0x00000018
OBJ_ATTR_KCV
0x59a46e
```

## Parameters<a name="genSymKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-t**  
Specifies the type of the symmetric key\. Enter the constant that represents the key type\. For example, to create an AES key, type `-t 31`\.  
Valid values:   

+ 16: [GENERIC\_SECRET](http://docs.oasis-open.org/pkcs11/pkcs11-curr/v2.40/cos01/pkcs11-curr-v2.40-cos01.html#_Toc408226962)\. A *generic secret key* is a byte array that does not conform to any particular standard, such as the requirements for an AES key\. 

+ 18: [RC4](https://en.wikipedia.org/wiki/RC4)\. RC4 keys are not valid on FIPS\-mode HSMs

+ 21: [Triple DES \(3DES\)](https://en.wikipedia.org/wiki/Triple_DES)\.

+ 31: [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
Required: Yes

**\-s**  
Specifies the key size in bytes\. For example, to create a 192\-bit key, type `24`\.   
Valid values for each key type:  

+ AES: 16 \(128 bits\), 24 \(192 bits\), 32 \(256 bits\)

+ 3DES: 24 \(192 bits\)

+ RC4: <256 \(2048 bits\)

+ Generic Secret: <3584 \(28672 bits\)
Required: Yes

**\-l**  
Specifies a user\-defined label for the key\. Type a string\.  
You can use any phrase that helps you to identify the key\. Because the label does not have to be unique, you can use it to group and categorize keys\.   
Required: Yes

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered with\.  
Default: No attestation check\.  
Required: No

**\-id**  
Specifies a user\-defined identifier for the key\. Type a string that is unique in the cluster\. The default is an empty string\.   
Default: No ID value\.  
Required: No

**\-min\_srv**  
Specifies the minimum number of HSMs on which the key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**\-m\_value**  
Specifies the number of users who must approve any cryptographic operation that uses the key\. Type a value from `0` to `8`\.  
This parameter establishes a quorum authentication requirement for the key\. The default value, `0`, disables the quorum authentication feature for the key\. When quorum authentication is enabled, the specified number of users must sign a token to approve cryptographic operations that use the key, and operations that share or unshare the key\.  
To find the `m_value` of a key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\.  
This parameter is valid only when the `-u` parameter in the command shares the key with enough users to satisfy the `m_value` requirement\.  
Default: 0  
Required: No

**\-nex**  
Makes the key nonextractable\. The key that is generated cannot be [exported from the HSM](manage-keys.md#export-keys)\.  
Default: The key is extractable\.  
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
Shares the key with the specified users\. This parameter gives other HSM crypto users \(CUs\) permission to use this key in cryptographic operations\.  
Type a comma\-separated list of HSM user IDs, such as \-`u 5,6`\. Do not include the HSM user ID of the current user\. To find HSM user IDs of CUs on the HSM, use [listUsers](key_mgmt_util-listUsers.md)\. To share and unshare existing keys, use shareKey\.   
Default: Only the current user can use the key\.   
Required: No

## Related Topics<a name="genSymKey-seealso"></a>

+ [exSymKey](key_mgmt_util-exSymKey.md)

+ [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md)

+ [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md)

+ [genECCKeyPair](key_mgmt_util-genECCKeyPair.md)