# getAttribute<a name="cloudhsm_mgmt_util-getAttribute"></a>

The getAttribute command in cloudhsm\_mgmt\_util gets one attribute value for a key from all HSMs in the cluster and writes it to stdout or to a file\. Only crypto users \(CUs\) can run this command\. 

*Key attributes* are properties of a key\. They include characteristics, like the key type, class, label, and ID, and values that represent actions that you can perform on the key, like encrypt, decrypt, wrap, sign, and verify\. 

You can use getAttribute only on keys that you own and key that are shared with you\. You can run this command or the [getAttribute](#cloudhsm_mgmt_util-getAttribute) command in key\_mgmt\_util, which writes one or all of the attribute values of a key to a file\. 

To get a list of attributes and the constants that represent them, use the [listAttributes](key_mgmt_util-listAttributes.md) command\. To change the attribute values of existing keys, use [setAttribute](key_mgmt_util-setAttribute.md) in key\_mgmt\_util and [setAttribute](cloudhsm_mgmt_util-setAttribute.md) in cloudhsm\_mgmt\_util\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="chmu-getAttribute-userType"></a>

The following types of users can run this command\.

+ Crypto users \(CU\)

## Syntax<a name="chmu-getAttribute-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto user \(CU\)

```
getAttribute <key handle> <attribute id> [<filename>]
```

## Example<a name="chmu-getAttribute-examples"></a>

This example gets the value of the extractable attribute for a key in the HSMs\. You can use a command like this to determine whether you can export a key from the HSMs\. 

The first command uses `listAttributes` to find the constant that represents the extractable attribute\. The output shows that the constant for `OBJ_ATTR_EXTRACTABLE` is `354`\. You can also find this information\. with descriptions of the attributes and their values, in the [Key Attribute Reference](key-attribute-table.md)\.

```
aws-cloudhsm> listAttributes

Following are the possible attribute values for getAttributes:

      OBJ_ATTR_CLASS                  = 0
      OBJ_ATTR_TOKEN                  = 1
      OBJ_ATTR_PRIVATE                = 2
      OBJ_ATTR_LABEL                  = 3
      OBJ_ATTR_KEY_TYPE               = 256
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

The second command uses getAttribute to get the value of the extractable attribute for the key with key handle `262170` in the HSMs\. To specify the extractable attribute, the command uses `354`, the constant that represents the attribute\. Because the command does not specify a file name, getAttribute writes the output to stdout\.

The output shows that the value of the extractable attribute is 1 on all of the HSM\. This value indicates that the owner of the key can export it\. When the value is 0 \(0x0\), it cannot be exported from the HSMs\. You set the value of the extractable attribute when you create a key, but you cannot change it\.

```
aws-cloudhsm> getAttribute 262170 354

Attribute Value on server 0(10.0.1.10):
OBJ_ATTR_EXTRACTABLE
0x00000001

Attribute Value on server 1(10.0.1.12):
OBJ_ATTR_EXTRACTABLE
0x00000001

Attribute Value on server 2(10.0.1.7):
OBJ_ATTR_EXTRACTABLE
0x00000001
```

## Arguments<a name="chmu-getAttribute-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
getAttribute <key handle> <attribute id> [<filename>]
```

**<key\-handle>**  
Specifies the key handle of the target key\. You can specify only one key in each command\. To get the key handle of a key, use [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util\.   
You must own the specified key or it must be shared with you\. To find the users of a key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md) in key\_mgmt\_util\.   
Required: Yes

**<attribute id>**  
Identifies the attribute\. Enter a constant that represents an attribute, or `512`, which represents all attributes\. For example, to get the key type, type `256`, which is the constant for the `OBJ_ATTR_KEY_TYPE` attribute\.  
To list the attributes and their constants, use [listAttributes](key_mgmt_util-listAttributes.md)\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
Required: Yes

**<filename>**  
Writes the output to the specified file\. Type a file path\.  
If the specified file exists, getAttribute overwrites the file without warning\.  
Required: No  
Default: Stdout

## Related Topics<a name="chmu-getAttribute-seealso"></a>

+ [getAttribute](key_mgmt_util-getAttribute.md) in key\_mgmt\_util

+ [listAttributes](cloudhsm_mgmt_util-listAttributes.md)

+ [setAttribute](cloudhsm_mgmt_util-setAttribute.md) in cloudhsm\_mgmt\_util

+ [setAttribute](key_mgmt_util-setAttribute.md) in key\_mgmt\_util

+ [Key Attribute Reference](key-attribute-table.md)