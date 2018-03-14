# imSymKey<a name="key_mgmt_util-imSymKey"></a>

The `imSymKey` command in the key\_mgmt\_util tool imports a plaintext copy of a symmetric key from a file into the HSM\. You can use it to import keys that you generate by any method outside of the HSM and keys that were exported from an HSM, such as the keys that the [exSymKey](key_mgmt_util-exSymKey.md), command writes to a file\. 

During the import process, `imSymKey` uses an AES key that you select \(the *wrapping key*\) to *wrap* \(encrypt\) and then *unwrap* \(decrypt\) the key to be imported\. However, `imSymKey` works only on files that contain plaintext keys\. To export and import encrypted keys, use the [wrapKey](key_mgmt_util-wrapKey.md) and [unWrapKey](key_mgmt_util-unwrapKey.md) commands\.

Also, the `imSymKey` command exports only symmetric keys\. To import public keys, use `importPubKey`\. To import private keys, use `importPrivateKey` or `wrapKey`\.

Imported keys work very much like keys generated in the HSM\. However, the value of the [OBJ\_ATTR\_LOCAL attribute](key-attribute-table.md) is zero, which indicates that it was not generated locally\. The `imSymKey` command does not have a parameter that shares the key with other users, but you can use the `shareKey` command in [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) to share the key after it is imported\. 

After you import a key, be sure to mark or delete the key file\. This command does not prevent you from importing the same key material multiple times\. The result, multiple keys with distinct key handles and the same key material, make it difficult to track use of the key material and prevent it from exceeding its cryptographic limits\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="imSymKey-syntax"></a>

```
imSymKey -h

imSymKey -f <key-file>
         -w <wrapping-key-handle>  
         -t <key-type>
         -l <label>
         [-id <key-ID>]
         [-sess]
         [-wk <wrapping-key-file> ]
         [-attest]
         [-min_srv <minimum-number-of-servers>]
         [-timeout <number-of-seconds> ]
```

## Examples<a name="imSymKey-examples"></a>

These examples show how to use `imSymKey` to import symmetric keys into your HSMs\.

**Example : Import an AES Symmetric Key**  
This example uses `imSymKey` to import an AES symmetric key into the HSMs\.   
The first command uses OpenSSL to generate a random 256\-bit AES symmetric key\. It saves the key in the `aes256.key` file\.  

```
$  openssl rand -out aes256-forImport.key 32
```
The second command uses `imSymKey` to import the AES key from the `aes256.key` file into the HSMs\. It uses key 20, an AES key in the HSM, as the wrapping key and it specifies a label of `imported`\. Unlike the ID, the label does not need to be unique in the cluster\. The value of the `-t` \(type\) parameter is `31`, which represents AES\.   
The output shows that the key in the file was wrapped and unwrapped, then imported into the HSM, where it was assigned the key handle 262180\.  

```
Command:  imSymKey -f aes256.key -w 20 -t 31 -l imported

        Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS

        Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS

        Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Unwrapped.  Key Handle: 262180

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```
The next command uses [getAttribute](key_mgmt_util-getAttribute.md) to get the OBJ\_ATTR\_LOCAL attribute \([attribute 355](key-attribute-table.md)\) of the newly imported key and writes it to the `attr_262180` file\.  

```
Command:  getAttribute -o 262180 -a 355 -out attributes/attr_262180
Attributes dumped into attributes/attr_262180_imported file

        Cfm3GetAttribute returned: 0x00 : HSM Return: SUCCESS
```
When you examine the attribute file, you can see that the value of the `OBJ_ATTR_LOCAL` attribute is zero, which indicates that the key material was not generated in the HSM\.   

```
$  cat attributes/attr_262180_local
OBJ_ATTR_LOCAL
0x00000000
```

**Example : Move a Symmetric Key Between Clusters**  
This example shows how to use `exSymKey` and `imSymKey` to move a plaintext AES key between clusters\. You might use a process like this one to create an AES wrapping that exists on the HSMs both clusters\. Once the shared wrapping key is in place, you can use [wrapKey](key_mgmt_util-wrapKey.md) and [unWrapKey](key_mgmt_util-unwrapKey.md) to move encrypted keys between the clusters\.  
The CU user who performs this operation must have permission to log in to the HSMs on both clusters\.  
The first command uses `exSymKey` to export key 14, a 32\-bit AES key, from the cluster 1 into the `aes.key` file\. It uses key 6, an AES key on the HSMs in cluster 1, as the wrapping key\.   

```
Command: exSymKey -k 14 -w 6 -out aes.key

        Cfm3WrapKey returned: 0x00 : HSM Return: SUCCESS

        Cfm3UnWrapHostKey returned: 0x00 : HSM Return: SUCCESS


Wrapped Symmetric Key written to file "aes.key"
```
The user then logs into key\_mgmt\_util in cluster 2 and runs an `imSymKey` command to import the key in the `aes.key` file into the HSMs in cluster 2\. This command uses key 252152, an AES key on the HSMs in cluster 2, as the wrapping key\.   
Because the wrapping keys that `exSymKey` and `imSymKey` use wrap and immediately unwrap the target keys, the wrapping keys on the different clusters need not be the same\.   
The output shows that the key was successfully imported into cluster 2 and assigned a key handle of 21\.   

```
Command:  imSymKey -f aes.key -w 262152 -t 31 -l xcluster

        Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS

        Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS

        Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Unwrapped.  Key Handle: 21

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```
To prove that key 14 of cluster 1 and key 21 in cluster 2 have the same key material, get the key check value \(KCV\) of each key\. If the KCV values are the same, the key material is the same\.  
The following command uses [getAttribute](key_mgmt_util-getAttribute.md) in cluster 1 to write the value of the KCV attribute \(attribute 371\) of key 14 to the `attr_14_kcv` file\. Then, it uses a cat command to get the content of the `attr_14_kcv` file\.  

```
Command:  getAttribute -o 14 -a 371 -out attr_14_kcv
Attributes dumped into attr_14_kcv file

$  cat attr_14_kcv
OBJ_ATTR_KCV
0xc33cbd
```
This similar command uses [getAttribute](key_mgmt_util-getAttribute.md) in cluster 2 to write the value of the KCV attribute \(attribute 371\) of key 21 to the `attr_21_kcv` file\. Then, it uses a cat command to get the content of the `attr_21_kcv` file\.  

```
Command:  getAttribute -o 21 -a 371 -out attr_21_kcv
Attributes dumped into attr_21_kcv file

$  cat attr_21_kcv
OBJ_ATTR_KCV
0xc33cbd
```
The output shows that the KCV values of the two keys are the same, which proves that the key material is the same\.  
Because the same key material exists in the HSMs of both clusters, you can now share encrypted keys between the clusters without ever exposing the plaintext key\. For example, you can use the `wrapKey` command with wrapping key 14 to export an encrypted key from cluster 1, and then use `unWrapKey` with wrapping key 21 to import the encrypted key into cluster 2\.

**Example : Import a Session Key**  
This command uses the `-sess` parameters of `imSymKey` to import a 192\-bit Triple DES key that is valid only in the current session\.   
The command uses the `-f` parameter to specify he file that contains the key to import, the `-t` parameter to specify the key type, and the `-w` parameter to specify the wrapping key\. It uses the `-l` parameter to specify a label that categorizes the key and the `-id` parameter to create a friendly, but unique, identifier for the key\. It also uses the `-attest` parameter to verify the firmware that is importing the key\.   
The output shows that the key was successfully wrapped and unwrapped, imported into the HSM, and assigned the key handle 37\. Also, the attestation check passed, which indicates that the firmware has not been tampered\.  

```
Command:  imSymKey -f 3des192.key -w 6 -t 21 -l temp -id test01 -sess -attest

        Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS

        Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS

        Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Unwrapped.  Key Handle: 37

        Attestation Check : [PASS]

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
Next, you can use the [getAttribute](key_mgmt_util-getAttribute.md) or [findKey](key_mgmt_util-findKey.md) commands to verify the attributes of the newly imported key\. The following command uses `findKey` to verify that key 37 has the type, label, and ID specified by the command, and that it is a session key\. A shown on line 5 of the output, `findKey` reports that the only key that matches all of the attributes is key 37\.   

```
Command:  findKey -t 21 -l temp -id test01 -sess 1
Total number of keys present 1

 number of keys matched from start index 0::0
37

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="imSymKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-f**  
Specifies the file that contains that key to import\.  
The file must contain a plaintext copy of an AES or Triple DES key of the specified length\. RC4 and DES keys are not valid on FIPS\-mode HSMs\.  

+ **AES**: 16, 24 or 32 bytes

+ **Triple DES \(3DES\)**: 24 bytes
Required: Yes

**\-w**  
Specifies the key handle of the wrapping key\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
A *wrapping key* is a key in the HSM that is used to encrypt \("wrap"\) and then decrypt \("unwrap\) the key during the import process\. Only AES keys can be used as wrapping keys\.  
You can use any AES key \(of any size\) as a wrapping key\. Because the wrapping key wraps, and then immediately unwraps, the target key, you can use as session\-only AES key as a wrapping key\. To determine whether a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute \(262\)\. To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\.  
If you use the `-wk` parameter to specify an external wrapping key, the `-w` wrapping key is used to unwrap, but not to wrap, the key that is being imported\.  
Key 4 is an unsupported internal key\. We recommend that you use an AES key that you create and manage as the wrapping key\.
Required: Yes

**\-t**  
Specifies the type of the symmetric key\. Enter the constant that represents the key type\. For example, to create an AES key, enter `-t 31`\.  
Valid values:   

+ 21: [Triple DES \(3DES\)](https://en.wikipedia.org/wiki/Triple_DES)\.

+ 31: [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
Required: Yes

**\-l**  
Specifies a user\-defined label for the key\. Type a string\.  
You can use any phrase that helps you to identify the key\. Because the label does not have to be unique, you can use it to group and categorize keys\.   
Required: Yes

**\-id**  
Specifies a user\-defined identifier for the key\. Type a string that is unique in the cluster\. The default is an empty string\.   
Default: No ID value\.  
Required: No

**\-sess**  
Creates a key that exists only in the current session\. The key cannot be recovered after the session ends\.  
Use this parameter when you need a key only briefly, such as a wrapping key that encrypts, and then quickly decrypts, another key\. Do not use a session key to encrypt data that you might need to decrypt after the session ends\.  
To change a session key to a persistent \(token\) key, use [setAttribute](key_mgmt_util-setAttribute.md)\.  
Default: The key is persistent\.   
Required: No

**\-wk**  
Use the AES key in the specified file to wrap the key that is being imported\. Enter the path and name of a file that contains a plaintext AES key\.   
When you include this parameter\. `imSymKey` uses the key in the `-wk` file to wrap the key being imported and it uses the key in the HSM that is specified by the `-w` parameter to unwrap it\. The `-w` and `-wk` parameter values must resolve to the same plaintext key\.  
Default: Use the wrapping key on the HSM to unwrap\.  
Required: No

**\-attest**  
Runs an integrity check that verifies that the firmware on which the cluster runs has not been tampered with\.  
Default: No attestation check\.  
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

## Related Topics<a name="imSymKey-seealso"></a>

+ [genSymKey](key_mgmt_util-genSymKey.md)

+ [exSymKey](key_mgmt_util-exSymKey.md)

+ [wrapKey](key_mgmt_util-wrapKey.md)

+ [unWrapKey](key_mgmt_util-unwrapKey.md)

+ exportPrivateKey

+ exportPubKey