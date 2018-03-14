# wrapKey<a name="key_mgmt_util-wrapKey"></a>

The `wrapKey` command in key\_mgmt\_util exports an encrypted copy of a symmetric or private key from the HSM to a file on disk\. When you run `wrapKey`, you specify the key to export, an AES key on the HSM to encrypt \(wrap\) the key to be exported, and the output file\.

The `wrapKey` command writes the encrypted key to a file that you specify, but it does not remove the key from the HSM, change its [key attributes](key-attribute-table.md), or prevent you from using it in cryptographic operations\. You can export the same key multiple times\. 

Only the owner of a key, that is, the CU user who created the key, can export it\. Users who share the key can use it in cryptographic operations, but they cannot export it\.

To import \(and unwrap\) the encrypted key from the file to an HSM, use [unWrapKey](key_mgmt_util-unwrapKey.md)\. To export a plaintext key from the HSM, use [exSymKey](key_mgmt_util-exSymKey.md)\. The [aesWrapUnwrap](key_mgmt_util-aesWrapUnwrap.md) command cannot decrypt \(unwrap\) keys that wrapKey encrypts\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="wrapKey-syntax"></a>

```
wrapKey -h

wrapKey -k <exported-key-handle>
        -w <wrapping-key-handle>
        -out <output-file>
```

## Example<a name="wrapKey-examples"></a>

**Example**  
This command exports a 192\-bit Triple DES \(3DES\) symmetric key \(key handle `7`\)\. It uses a 256\-bit AES key in the HSM \(key handle `14`\) to wrap key `7`\. Then it writes the encrypted 3DES key to the `3DES-encrypted.key` file\.  
The output shows that key `7` \(the 3DES key\) was successfully wrapped and written to the specified file\. The encrypted key is 307 bytes long\.  

```
        Command:  wrapKey -k 7 -w 14 -out 3DES-encrypted.key

        Key Wrapped.

        Wrapped Key written to file "3DES-encrypted.key length 307

        Cfm2WrapKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="wrapKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-k**  
Specifies the key handle of the key to export\. Type the key handle of a symmetric or private key that you own\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To verify that a key can be exported, use the [getAttribute](key_mgmt_util-getAttribute.md) command to get the value of the `OBJ_ATTR_EXTRACTABLE` attribute, which is represented by constant `354`\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
Also, you can export only keys that you own\. To find the owner of a key, use the [getKeyInfo](key_mgmt_util-getKeyInfo.md) command\.  
Required: Yes

**\-w**  
Specifies the wrapping key\. Type the key handle of an AES key on the HSM\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\. To verify that a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute, which is represented by constant `262`\.  
Key handle 4 represents an unsupported internal key\. We recommend that you use an AES key that you create and manage as the wrapping key\.
Required: Yes

**\-out**  
Specifies the path and name of the output file\. When the command succeeds, this file contains an encrypted copy of the exported key\. If the file already exists, the command overwrites it without warning\.  
Required: Yes

## Related Topics<a name="wrapKey-seealso"></a>

+ [exSymKey](key_mgmt_util-exSymKey.md)

+ [imSymKey](key_mgmt_util-imSymKey.md)

+ [unWrapKey](key_mgmt_util-unwrapKey.md)