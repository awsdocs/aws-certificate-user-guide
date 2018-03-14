# aesWrapUnwrap<a name="key_mgmt_util-aesWrapUnwrap"></a>

The aesWrapUnwrap command encrypts or decrypts the contents of a file on disk\. This command is designed to wrap and unwrap encryption keys, but you can use it on any file that contains less than 4 KB \(4096 bytes\) of data\.

aesWrapUnwrap uses [AES Key Wrap](https://tools.ietf.org/html/rfc3394)\. It uses an AES key on the HSM as the wrapping or unwrapping key\. Then it writes the result to another file on disk\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="aesWrapUnwrap-syntax"></a>

```
aesWrapUnwrap -h

aesWrapUnwrap -m <wrap-unwrap mode>
              -f <file-to-wrap-unwrap> 
              -w <wrapping-key-handle>               
              [-i <wrapping-IV>] 
              [-out <output-file>]
```

## Examples<a name="aesWrapUnwrap-examples"></a>

These examples show how to use aesWrapUnwrap to encrypt and decrypt an encryption key in a file\. 

**Example : Wrap an Encryption Key**  
This command uses aesWrapUnwrap to wrap a Triple DES symmetric key that was [exported from the HSM in plaintext](key_mgmt_util-exSymKey.md) into the `3DES.key` file\. You can use a similar command to wrap any key saved in a file\.   
The command uses the `-m` parameter with a value of `1` to indicate wrap mode\. It uses the `-w` parameter to specify an AES key in the HSM \(key handle `6`\) as the wrapping key\. It writes the resulting wrapped key to the `3DES.key.wrapped` file\.  
The output shows that the command was successful and that the operation used the default IV, which is preferred\.  

```
 Command:  aesWrapUnwrap -f 3DES.key -w 6 -m 1 -out 3DES.key.wrapped

        Warning: IV (-i) is missing.
                 0xA6A6A6A6A6A6A6A6 is considered as default IV
result data:
49 49 E2 D0 11 C1 97 22
17 43 BD E3 4E F4 12 75
8D C1 34 CF 26 10 3A 8D
6D 0A 7B D5 D3 E8 4D C2
79 09 08 61 94 68 51 B7

result written to file 3DES.key.wrapped

        Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
```

**Example : Unwrap an Encryption Key**  
This example shows how to use aesWrapUnwrap to unwrap \(decrypt\) a wrapped \(encrypted\) key in a file\. You might want to do an operation like this one before importing a key to the HSM\. For example, if you try to use the [imSymKey](key_mgmt_util-imSymKey.md) command to import an encrypted key, it returns an error because the encrypted key doesn't have the format that is required for a plaintext key of that type\.  
The command unwraps the key in the `3DES.key.wrapped` file and writes the plaintext to the `3DES.key.unwrapped` file\. The command uses the `-m` parameter with a value of `0` to indicate unwrap mode\. It uses the `-w` parameter to specify an AES key in the HSM \(key handle `6`\) as the wrapping key\. It writes the resulting wrapped key to the `3DES.key.unwrapped` file\.   

```
 Command:  aesWrapUnwrap -m 0 -f 3DES.key.wrapped -w 6 -out 3DES.key.unwrapped

        Warning: IV (-i) is missing.
                 0xA6A6A6A6A6A6A6A6 is considered as default IV
result data:
14 90 D7 AD D6 E4 F5 FA
A1 95 6F 24 89 79 F3 EE
37 21 E6 54 1F 3B 8D 62

result written to file 3DES.key.unwrapped

        Cfm3UnWrapHostKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="aesWrapUnwrap-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-m**  
Specifies the mode\. To wrap \(encrypt\) the file content, type `1`; to unwrap \(decrypt\) the file content, type `0`\.  
Required: Yes

**\-f**  
Specifies the file to wrap\. Enter a file that contains less than 4 KB \(4096 bytes\) of data\. This operation is designed to wrap and unwrap encryption keys\.  
Required: Yes

**\-w**  
Specifies the wrapping key\. Type the key handle of an AES key on the HSM\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\. To verify that a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute, which is represented by constant `262`\.  
Key handle 4 represents an unsupported internal key\. We recommend that you use an AES key that you create and manage as the wrapping key\.
Required: Yes

**\-i**  
Specifies an alternate initial value \(IV\) for the algorithm\. Use the default value unless you have a special condition that requires an alternative\.  
Default: `0xA6A6A6A6A6A6A6A6`\. The default value is defined in the [AES Key Wrap](https://tools.ietf.org/html/rfc3394) algorithm specification\.  
Required: No

**\-out**  
Specifies an alternate name for the output file that contains the wrapped or unwrapped key\. The default is `wrapped_key` \(for wrap operations\) and `unwrapped_key` \(for unwrap operations\) in the local directory\.  
If the file exists, the aesWrapUnwrap overwrites it without warning\. If the command fails, aesWrapUnwrap creates an output file with no contents\.  
Default: For wrap: `wrapped_key`\. For unwrap: `unwrapped_key`\.  
Required: No

## Related Topics<a name="aesWrapUnwrap-seealso"></a>

+ [exSymKey](key_mgmt_util-exSymKey.md)

+ [imSymKey](key_mgmt_util-imSymKey.md)

+ [unWrapKey](key_mgmt_util-unwrapKey.md)

+ [wrapKey](key_mgmt_util-wrapKey.md)