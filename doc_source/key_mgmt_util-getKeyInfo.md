# getKeyInfo<a name="key_mgmt_util-getKeyInfo"></a>

The getKeyInfo command in the key\_mgmt\_util returns the HSM user IDs of users who can use the key, including the owner and crypto users \(CU\) with whom the key is shared\. When quorum authentication is enabled on a key, getKeyInfo also returns the number of users who must approve cryptographic operations that use the key\. You can run getKeyInfo only on keys that you own and keys that are shared with you\.

When you run getKeyInfo on public keys, getKeyInfo returns only the key owner, even though all users of the HSM can use the public key\. To find the HSM user IDs of users in your HSMs, use [listUsers](key_mgmt_util-listUsers.md)\. To find the keys for a particular user, use [findKey](key_mgmt_util-findKey.md) `-u`\.

You own the keys that you create\. You can share a key with other users when you create it\. Then, to share or unshare an existing key, use [shareKey](cloudhsm_mgmt_util-shareKey.md) in cloudhsm\_mgmt\_util\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="getKeyInfo-syntax"></a>

```
getKeyInfo -h

getKeyInfo -k <key-handle>
```

## Examples<a name="getKeyInfo-examples"></a>

These examples show how to use getKeyInfo to get information about the users of a key\.

**Example : Get the Users for a Symmetric Key**  
This command gets the users who can use the AES \(symmetric\) key with key handle `9`\. The output shows that user 3 owns the key and has shared it with user 4\.  

```
Command:  getKeyInfo -k 9

       Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

       Owned by user 3

       also, shared to following 1 user(s):

                4
```

**Example : Get the Users for an Asymmetric Key Pair**  
These commands use getKeyInfo to get the users who can use the keys in an RSA \(asymmetric\) key pair\. The public key has key handle `21`\. The private key has key handle `20`\.   
When you run getKeyInfo on the private key \(`20`\), it returns the key owner \(3\) and crypto users \(CUs\) 4 and 5, with whom the key is shared\.   

```
Command:  getKeyInfo -k 20

       Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

       Owned by user 3

       also, shared to following 2 user(s):

                4
                5
```
When you run getKeyInfo on the public key \(`21`\), it returns only the key owner \(3\)\.   

```
Command:  getKeyInfo -k 21

       Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

       Owned by user 3
```
To confirm that user 4 can use the public key \(and all public keys on the HSM\), use the `-u` parameter of [findKey](key_mgmt_util-findKey.md)\.   
The output shows that user 4 can use both the public \(`21`\) and private \(`20`\) key in the key pair\. User 4 can also use all other public keys and any private keys that they have created or that have been shared with them\.   

```
Command:  findKey -u 4
Total number of keys present 8

 number of keys matched from start index 0::7
11, 12, 262159, 262161, 262162, 19, 20, 21

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

**Example : Get the Quorum Authentication Value \(m\_value\) for a Key**  
This example shows how to get the `m_value` for a key, that is, the number of users in the quorum who must approve any cryptographic operations that use the key\.  
When quorum authentication is enabled on a key, a quorum of users must approve any cryptographic operations that use the key\. To enable quorum authentication and set the quorum size, use the `-m_value` parameter when you create the key\.  
This command uses [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md) to create an RSA key pair that is shared with user 4\. It uses the `m_value` parameter to enable quorum authentication on the private key in the pair and set the quorum size to two users\. The number of users must be large enough to provide the required approvals\.  
The output shows that the command created public key `27` and private key `28`\.  

```
 Command:  genRSAKeyPair -m 2048 -e 195193 -l rsa_mofn -id rsa_mv2 -u 4 -m_value 2

        Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

        Cfm3GenerateKeyPair:    public key handle: 27    private key handle: 28

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
```
This command uses getKeyInfo to get information about the users of the private key\. The output shows that the key is owned by user 3 and shared with user 4\. It also shows that a quorum of two users must approve every cryptographic operation that uses the key\.  

```
Command:  getKeyInfo -k 28

        Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

        Owned by user 3

        also, shared to following 1 user(s):

                 4
         2 Users need to approve to use/manage this key
```

## Parameters<a name="getKeyInfo-parameters"></a>

**\-h**  
Displays command line help for the command\.   
Required: Yes

**\-k**  
Specifies the key handle of one key in the HSM\. Enter the key handle of a key that you own or share\. This parameter is required\.   
To find key handles, use the [findKey](key_mgmt_util-listUsers.md) command\.  
Required: Yes

## Related Topics<a name="getKeyInfo-seealso"></a>

+ [getKeyInfo](cloudhsm_mgmt_util-getKeyInfo.md) in cloudhsm\_mgmt\_util

+ [listUsers](key_mgmt_util-listUsers.md)

+ [findKey](key_mgmt_util-findKey.md)

+ [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util