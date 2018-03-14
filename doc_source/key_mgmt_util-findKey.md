# findKey<a name="key_mgmt_util-findKey"></a>

Use the findKey command in key\_mgmt\_util to search for keys by the values of the key attributes\. When a key matches all the criteria that you set, findKey returns the key handle\. With no parameters, findKey returns the key handles of all the keys that you can use in the HSM\. To find the attribute values of a particular key, use [getAttribute](key_mgmt_util-getAttribute.md)\.

Like all key\_mgmt\_util commands, findKey is user specific\. It returns only the keys that the current user can use in cryptographic operations\. This includes keys that current user owns and keys that have been shared with the current user\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="findKey-syntax"></a>

```
findKey -h 

findKey [-c <key class>] 
        [-t <key type>]
        [-l <key label>] 
        [-id <key ID>]
        [-sess (0 | 1)] 
        [-u <user-ids>]
        [-m <modulus>]
        [-kcv <key_check_value>]
```

## Examples<a name="findKey-examples"></a>

These examples show how to use findKey to find and identify keys in your HSMs\.

**Example : Find All Keys**  
This command finds all keys for the current user in the HSM\. The output includes keys that the user owns and shares, and all public keys in the HSMs\.  
To get the attributes of a key with a particular key handle, use [getAttribute](key_mgmt_util-getAttribute.md)\. To determine whether the current user owns or shares a particular key, use [getKeyInfo](key_mgmt_util-getKeyInfo.md) or [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util\.  

```
Command: findKey

Total number of keys present 13

 number of keys matched from start index 0::12
6, 7, 524296, 9, 262154, 262155, 262156, 262157, 262158, 262159, 262160, 262161, 262162

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

**Example : Find Keys by Type, User, and Session**  
This command finds persistent AES keys that the current user and user 3 can use\. \(User 3 might be able to use other keys that the current user cannot see\.\)  

```
Command: findKey -t 31 -sess 0 -u 3
```

**Example : Find Keys by Class and Label**  
This command finds all public keys for the current user with the `2018-sept` label\.  

```
Command: findKey -c 2 -l 2018-sept
```

**Example : Find RSA Keys by Modulus**  
This command finds RSA keys \(type 0\) for the current user that were created by using the modulus in the `m4.txt` file\.  

```
Command: findKey -t 0 -m m4.txt
```

## Parameters<a name="findKey-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-t**  
Finds keys of the specified type\. Enter the constant that represents the key class\. For example, to find 3DES keys, type `-t 21`\.  
Valid values:   

+ 0: [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))

+ 1: [DSA](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm)

+ 3: [EC](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography)

+ 16: [GENERIC\_SECRET](http://docs.oasis-open.org/pkcs11/pkcs11-curr/v2.40/cos01/pkcs11-curr-v2.40-cos01.html#_Toc408226962)

+ 18: [RC4](https://en.wikipedia.org/wiki/RC4)

+ 21: [Triple DES \(3DES\)](https://en.wikipedia.org/wiki/Triple_DES)

+ 31: [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
Required: No

**\-c**  
Finds keys in the specified class\. Enter the constant that represents the key class\. For example, to find public keys, type `-c 2`\.  
Valid values for each key type:  

+ 2: Public\. This class contains the public keys of public–private key pairs\.

+ 3: Private\. This class contains the private keys of public–private key pairs\.

+ 4: Secret\. This class contains all symmetric keys\.
Required: No

**\-l**  
Finds keys with the specified label\. Type the exact label\. You cannot use wildcard characters or regular expressions in the `--label` value\.  
Required: No

**\-id**  
Finds the key with the specified ID\. Type the exact ID string\. You cannot use wildcard characters or regular expressions in the `-id` value\.  
Required: No

**\-sess**  
Finds keys by session status\. To find keys that are valid only in the current session, type `1`\. To find persistent keys, type `0`\.  
Required: No

**\-u**  
Finds keys the specified users and the current user share\. Type a comma\-separated list of HSM user IDs, such as `-u 3` or `-u 4,7`\. To find the IDs of users on an HSM, use [listUsers](key_mgmt_util-listUsers.md)\.  
When you specify one user ID, findKey returns the keys for that user\. When you specify multiple user IDs, findKey returns the keys that all the specified users can use\.  
Because findKey only returns keys that the current user can use, the `-u` results are always identical to or a subset of the current user's keys\. To get all keys that are owned by or shared with any user, crypto officers \(COs\) can use [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util\.  
Required: No

**\-m**  
Finds keys that were created by using the RSA modulus in the specified file\. Type the path to file that stores the modulus\.  
Required: No

**\-kcv**  
Finds keys with the specified key check value\.  
The *key check value* \(KCV\) is an 8\-byte hash or checksum of a key\. The HSM calculates a KCV when it generates the key\. You can also calculate a KCV outside of the HSM, such as after you export a key\. You can then compare the KCV values to confirm the identity and integrity of the key\. To get the KCV of a key, use [getAttribute](key_mgmt_util-getAttribute.md)\.  
AWS CloudHSM uses the following standard method to generate a key check value:  

+ **Symmetric keys**: First 8 bytes of the result of encrypting 16 zero\-filled bytes with the key\.

+ **Asymmetric key pairs**: First 8 bytes of the modulus hash\.
Required: No

## Output<a name="findKey-output"></a>

The findKey output lists the total number of matching keys and their key handles\.

```
        Command:  findKey
Total number of keys present 10

 number of keys matched from start index 0::9
6, 7, 8, 9, 10, 11, 262156, 262157, 262158, 262159

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

## Related Topics<a name="findKey-seealso"></a>

+ [findSingleKey](key_mgmt_util-findSingleKey.md)

+ [getKeyInfo](key_mgmt_util-getKeyInfo.md)

+ [getAttribute](key_mgmt_util-getAttribute.md)

+ [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util

+ [Key Attribute Reference](key-attribute-table.md)