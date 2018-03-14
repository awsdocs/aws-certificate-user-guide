# listAttributes<a name="key_mgmt_util-listAttributes"></a>

The listAttributes command in key\_mgmt\_util lists the attributes of an AWS CloudHSM key and the constants that represent them\. You use these constants to identify the attributes in [getAttribute](key_mgmt_util-getAttribute.md) and [setAttribute](key_mgmt_util-setAttribute.md) commands\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="listAttributes-syntax"></a>

This command has no parameters\.

```
listAttributes
```

## Example<a name="listAttributes-examples"></a>

This command lists the key attributes that you can get and change in key\_mgmt\_util and the constants that represent them\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\. 

To represent all attributes in the [getAttribute](key_mgmt_util-getAttribute.md) command in key\_mgmt\_util, use `512`\.

```
Command: listAttributes

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

## Related Topics<a name="listAttributes-seealso"></a>

+ [listAttributes](cloudhsm_mgmt_util-listAttributes.md) in cloudhsm\_mgmt\_util

+ [getAttribute](key_mgmt_util-getAttribute.md)

+ [setAttribute](key_mgmt_util-setAttribute.md)

+ [Key Attribute Reference](key-attribute-table.md)