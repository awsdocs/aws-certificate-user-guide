# setAttribute<a name="cloudhsm_mgmt_util-setAttribute"></a>

The setAttribute command in cloudhsm\_mgmt\_util changes the value of the label, encrypt, decrypt, wrap, and unwrap attributes of a key in the HSMs\. You can also use the [setAttribute](key_mgmt_util-setAttribute.md) command in key\_mgmt\_util to convert a session key to a persistent key\. You can only change the attributes of keys that you own\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="setAttribute-userType"></a>

The following types of users can run this command\.

+ Crypto users \(CU\)

## Syntax<a name="chmu-setAttribute-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto user \(CU\)

```
setAttribute <key handle> <attribute id>
```

## Example<a name="chmu-setAttribute-examples"></a>

This example shows how to disable the decrypt functionality of a symmetric key\. You can use a command like this one to configure a wrapping key, which should be able to wrap and unwrap other keys, but not to encrypt or decrypt data\.

The first step is to create the wrapping key\. This command uses [genSymKey](key_mgmt_util-genSymKey.md) in key\_mgmt\_util to generate a 256\-bit AES symmetric key\. The output shows that the new key has key handle 14\.

```
$  genSymKey -t 31 -s 32 -l aes256

Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Created.  Key Handle: 14

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

Next, we want to confirm the current value of the decrypt attribute\. To get the attribute ID of the decrypt attribute, use [listAttributes](cloudhsm_mgmt_util-listAttributes.md)\. The output shows that the constant that represents the `OBJ_ATTR_DECRYPT` attribute is `261`\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

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

To get the current value of the decrypt attribute for key 14, the next command uses [getAttribute](cloudhsm_mgmt_util-getAttribute.md) in cloudhsm\_mgmt\_util\. 

The output shows that the value of the decrypt attribute is true \(1\) on both HSMs in the cluster\.

```
aws-cloudhsm> getAttribute 14 261
      
Attribute Value on server 0(10.0.0.1):
OBJ_ATTR_DECRYPT
0x00000001

Attribute Value on server 1(10.0.0.2):
OBJ_ATTR_DECRYPT
0x00000001
```

This command uses setAttribute to change the value of the decrypt attribute \(attribute `261`\) of key 14 to `0`\. This will disable the decrypt functionality on the key\. 

The output shows that the command succeeded on both HSMs in the cluster\.

```
aws-cloudhsm> setAttribute 14 261 0
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)? y
setAttribute success on server 0(10.0.0.1)
setAttribute success on server 1(10.0.0.2)
```

The final command repeats the getAttribute command\. Again, it gets the decrypt attribute \(attribute `261`\) of key 14\.

This time, the output shows that the value of the decrypt attribute is false \(0\) on both HSMs in the cluster\.

```
aws-cloudhsm>getAttribute 14 261
Attribute Value on server 0(10.0.3.6):
OBJ_ATTR_DECRYPT
0x00000000

Attribute Value on server 1(10.0.1.7):
OBJ_ATTR_DECRYPT
0x00000000
```

## Arguments<a name="chmu-setAttribute-parameters"></a>

```
setAttribute <key handle> <attribute id>
```

**<key\-handle>**  
Specifies the key handle of a key that you own\. You can specify only one key in each command\. To get the key handle of a key, use [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util\. To find the users of a key, use [getKeyInfo](cloudhsm_mgmt_util-getKeyInfo.md)\.  
Required: Yes

**<attribute id>**  
Specifies the constant that represents the attribute that you want to change\. You can specify only one attribute in each command\. To get the attributes and their integer values, use [listAttributes](key_mgmt_util-listAttributes.md)\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
Valid values:  

+ **3**: `OBJ_ATTR_LABEL`\.

+ **260**: `OBJ_ATTR_ENCRYPT`\.

+ **261**: `OBJ_ATTR_DECRYPT`\.

+ **262**: `OBJ_ATTR_WRAP`\.

+ **263**: `OBJ_ATTR_UNWRAP`\.
Required: Yes

## Related Topics<a name="chmu-setAttribute-seealso"></a>

+ [setAttribute](key_mgmt_util-setAttribute.md) in key\_mgmt\_util

+ [getAttribute](cloudhsm_mgmt_util-getAttribute.md)

+ [listAttributes](cloudhsm_mgmt_util-listAttributes.md)

+ [Key Attribute Reference](key-attribute-table.md)