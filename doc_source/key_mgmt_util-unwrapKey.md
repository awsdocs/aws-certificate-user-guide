# unWrapKey<a name="key_mgmt_util-unwrapKey"></a>

The unWrapKey command in the key\_mgmt\_util tool imports a wrapped \(encrypted\) symmetric or private key from a file into the HSM\. It is designed to import encrypted keys from files that were created by the [wrapKey](key_mgmt_util-wrapKey.md) command\.

During the import process, unWrapKey uses an AES key on the HSM that you specify to unwrap \(decrypt\) the key in the file\. Then it saves the key in the HSM with a key handle and the attributes that you specify\. To export and import plaintext keys, use the [exSymKey](key_mgmt_util-exSymKey.md) and [imSymKey](key_mgmt_util-imSymKey.md) commands\.

Imported keys work very much like keys generated in the HSM\. However, the value of the [OBJ\_ATTR\_LOCAL attribute](key-attribute-table.md) is zero, which indicates that it was not generated locally\. The `unWrapKey` command does not have parameters that assign a label or share the key with other users, but you can use the `shareKey` command in [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) to add those attributes after the key is imported\. 

After you import a key, be sure to mark or delete the key file\. This command does not prevent you from importing the same key material multiple times\. The result, multiple keys with distinct key handles and the same key material, make it difficult to track use of the key material and prevent it from exceeding its cryptographic limits\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="unwrapKey-syntax"></a>

```
unWrapKey -h

unWrapKey -f <key-file-name> 
          -w <wrapping-key-handle> 4
          [-sess]
          [-min_srv <minimum-number-of-HSMs>]          
          [-timeout <number-of-seconds> ]
          [-attest]
```

## Example<a name="unwrapKey-examples"></a>

**Example**  
This command imports an wrapped \(encrypted\) copy of a Triple DES \(3DES\) symmetric key from the `3DES.key` file into the HSMs\. To unwrap \(decrypt\) the key, the command uses the `-w` parameter to specify key `6`, an AES key on the HSM\. The AES key that unwraps during import must be the same key that wrapped during export, or a cryptographically identical copy\.   
The output shows that the key in the file was unwrapped and imported\. The new key has key handle `29`\.   
If you are using unWrapKey to move a key between clusters, you must first create an AES wrapping key that exists on both clusters\. You can generate a key outside of the HSMs and then use `imSymKey` to import it to the HSMs on both cluster\. Or, generate an AES key in the HSMs on one cluster, use [exSymKey](key_mgmt_util-exSymKey.md) to export it in plaintext to a file\. Then use `imSymKey` to import the plaintext key into the other cluster\. Once the wrapping key is established on both clusters, you can use wrapKey and unWrapKey to move encrypted keys between clusters without ever exposing the plaintext key\.  

```
        Command:  unWrapKey -f 3DES.key -w 6

        Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS

        Key Unwrapped.  Key Handle: 29

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="unwrapKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-f**  
Specifies the path and name of the file that contains the wrapped key\.   
Required: Yes

**\-w**  
Specifies the wrapping key\. Type the key handle of an AES key on the HSM\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\. To verify that a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute, which is represented by constant `262`\.  
Key handle 4 represents an unsupported internal key\. We recommend that you use an AES key that you create and manage as the wrapping key\.
Required: Yes

**\-sess**  
Creates a key that exists only in the current session\. The key cannot be recovered after the session ends\.  
Use this parameter when you need a key only briefly, such as a wrapping key that encrypts, and then quickly decrypts, another key\. Do not use a session key to encrypt data that you might need to decrypt after the session ends\.  
To change a session key to a persistent \(token\) key, use [setAttribute](key_mgmt_util-setAttribute.md)\.  
Default: The key is persistent\.   
Required: No

**\-min\_srv**  
Specifies the minimum number of HSMs on which the key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**\-timeout**  
Specifies how long \(in seconds\) the command waits for a key to be synchronized to the number of HSMs specified by the `min_srv` parameter\.   
This parameter is valid only when the `min_srv` parameter is also used in the command\.  
Default: No timeout\. The command waits indefinitely and returns only when the key is synchronized to the minimum number of servers\.  
Required: No

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered with\.  
Default: No attestation check\.  
Required: No

## Related Topics<a name="unwrapKey-seealso"></a>

+ [wrapKey](key_mgmt_util-wrapKey.md)

+ [exSymKey](key_mgmt_util-exSymKey.md)

+ [imSymKey](key_mgmt_util-imSymKey.md)