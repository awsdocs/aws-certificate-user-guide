# getAttribute<a name="key_mgmt_util-getAttribute"></a>

The getAttribute command in key\_mgmt\_util writes one or all of the attribute values for an AWS CloudHSM key to a file\. If the attribute you specify does not exist for the key type, such as the modulus of an AES key, getAttribute returns an error\. 

*Key attributes* are properties of a key\. They include characteristics, like the key type, class, label, and ID, and values that represent actions that you can perform with the key, like encrypt, decrypt, wrap, sign, and verify\. 

You can use getAttribute only on keys that you own and key that are shared with you\. You can run this command or the [getAttribute](cloudhsm_mgmt_util-getAttribute.md) command in cloudhsm\_mgmt\_util, which gets one attribute value of a key from all HSMs in a cluster, and writes it to stdout or to a file\. 

To get a list of attributes and the constants that represent them, use the [listAttributes](key_mgmt_util-listAttributes.md) command\. To change the attribute values of existing keys, use [setAttribute](key_mgmt_util-setAttribute.md) in key\_mgmt\_util and [setAttribute](cloudhsm_mgmt_util-setAttribute.md) in cloudhsm\_mgmt\_util\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="getAttribute-syntax"></a>

```
getAttribute -h 

getAttribute -o <key handle> 
             -a <attribute constant> 
             -out <file>
```

## Examples<a name="getAttribute-examples"></a>

These examples show how to use getAttribute to get the attributes of keys in your HSMs\.

**Example : Get the Key Type**  
This example gets the type of the key, such an AES, 3DES, or generic key, or an RSA or elliptic curve key pair\.  
The first command runs [listAttributes](key_mgmt_util-listAttributes.md), which gets the key attributes and the constants that represent them\. The output shows that the constant for key type is `256`\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  

```
Command: listAttributes

Description
===========
The following are all of the possible attribute values for getAttributes.

      OBJ_ATTR_CLASS                  = 0
      OBJ_ATTR_TOKEN                  = 1
      OBJ_ATTR_PRIVATE                = 2
      OBJ_ATTR_LABEL                  = 3
      OBJ_ATTR_KEY_TYPE               = 256
      OBJ_ATTR_ID                     = 258
      OBJ_ATTR_SENSITIVE              = 259
      OBJ_ATTR_ENCRYPT                = 260
      OBJ_ATTR_DECRYPT                = 261
      OBJ_ATTR_WRAP                   = 262
      OBJ_ATTR_UNWRAP                 = 263
      OBJ_ATTR_SIGN                   = 264
      OBJ_ATTR_VERIFY                 = 266
      OBJ_ATTR_LOCAL                  = 355
      OBJ_ATTR_MODULUS                = 288
      OBJ_ATTR_MODULUS_BITS           = 289
      OBJ_ATTR_PUBLIC_EXPONENT        = 290
      OBJ_ATTR_VALUE_LEN              = 353
      OBJ_ATTR_EXTRACTABLE            = 354
      OBJ_ATTR_KCV                    = 371
```
The second command runs getAttribute\. It requests the key type \(attribute `256`\) for key handle `524296` and writes it to the `attribute.txt` file\.   

```
Command: getAttribute -o 524296 -a 256 -out attribute.txt
Attributes dumped into attribute.txt file
```
The final command gets the content of the key file\. The output reveals that the key type is `0x15` or `21`, which is a Triple DES \(3DES\) key\. For definitions of the class and type values, see the [Key Attribute Reference](key-attribute-table.md)\.  

```
$  cat attribute.txt
OBJ_ATTR_KEY_TYPE
0x00000015
```

**Example : Get All Attributes of a Key**  
This command gets all attributes of the key with key handle `6` and writes them to the `attr_6` file\. It uses an attribute value of `512`, which represents all attributes\.   

```
Command: getAttribute -o 6 -a 512 -out attr_6
        
got all attributes of size 444 attr cnt 17
Attributes dumped into attribute.txt file

        Cfm3GetAttribute returned: 0x00 : HSM Return: SUCCESS>
```
This command shows the content of a sample attribute file with all attribute values\. Among the values, it reports that key is a 256\-bit AES key with an ID of `test_01` and a label of `aes256`\.The key is extractable and persistent, that is, not a session\-only key\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  

```
$  cat attribute.txt

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
0x01
OBJ_ATTR_UNWRAP
0x01
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
aes256
OBJ_ATTR_ID
test_01
OBJ_ATTR_VALUE_LEN
0x00000020
OBJ_ATTR_KCV
0x1a4b31
```

## Parameters<a name="getAttribute-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-o**  
Specifies the key handle of the target key\. You can specify only one key in each command\. To get the key handle of a key, use [findKey](key_mgmt_util-findKey.md)\.  
Also, you must own the specified key or it must be shared with you\. To find the users of a key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md)\.  
Required: Yes

**\-a**  
Identifies the attribute\. Enter a constant that represents an attribute, or `512`, which represents all attributes\. For example, to get the key type, type `256`, which is the constant for the `OBJ_ATTR_KEY_TYPE` attribute\.  
To list the attributes and their constants, use [listAttributes](key_mgmt_util-listAttributes.md)\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
Required: Yes

**\-out**  
Writes the output to the specified file\. Type a file path\. You cannot write the output to `stdout`\.   
If the specified file exists, getAttribute overwrites the file without warning\.  
Required: Yes

## Related Topics<a name="getAttribute-seealso"></a>

+ [getAttribute](cloudhsm_mgmt_util-getAttribute.md) in cloudhsm\_mgmt\_util

+ [listAttributes](key_mgmt_util-listAttributes.md)

+ [setAttribute](key_mgmt_util-setAttribute.md)

+ [findKey](key_mgmt_util-findKey.md)

+ [Key Attribute Reference](key-attribute-table.md)