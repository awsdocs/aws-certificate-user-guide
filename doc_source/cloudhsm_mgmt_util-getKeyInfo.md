# getKeyInfo<a name="cloudhsm_mgmt_util-getKeyInfo"></a>

The getKeyInfo command in the key\_mgmt\_util returns the HSM user IDs of users who can use the key, including the owner and crypto users \(CU\) with whom the key is shared\. When quorum authentication is enabled on a key, getKeyInfo also returns the number of users who must approve cryptographic operations that use the key\. You can run getKeyInfo only on keys that you own and keys that are shared with you\.

When you run getKeyInfo on public keys, getKeyInfo returns only the key owner, even though all users of the HSM can use the public key\. To find the HSM user IDs of users in your HSMs, use [listUsers](key_mgmt_util-listUsers.md)\. To find the keys for a particular user, use [findKey](key_mgmt_util-findKey.md) `-u` in key\_mgmt\_util\. Crypto officers can use [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util\.

You own the keys that you create\. You can share a key with other users when you create it\. Then, to share or unshare an existing key, use [shareKey](cloudhsm_mgmt_util-shareKey.md) in cloudhsm\_mgmt\_util\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="chmu-getKeyInfo-userType"></a>

The following types of users can run this command\.

+ Crypto users \(CU\)

## Syntax<a name="chmu-getKeyInfo-syntax"></a>

```
getKeyInfo -k <key-handle> <output file>
```

## Examples<a name="chmu-getKeyInfo-examples"></a>

These examples show how to use getKeyInfo to get information about the users of a key\.

**Example : Get the Users for an Asymmetric Key**  
This command gets the users who can use the AES \(asymmetric\) key with key handle `262162`\. The output shows that user 3 owns the key and has shares it with users 4 and 6\.   
Only users 3, 4, and 6 can run getKeyInfo on key 262162\.   

```
aws-cloudhsm>getKeyInfo 262162
Key Info on server 0(10.0.0.1):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 2 user(s):

                 4
                 6
Key Info on server 1(10.0.0.2):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 2 user(s):

                 4
                 6
```

**Example : Get the Users for a Symmetric Key Pair**  
These commands use getKeyInfo to get the users who can use the keys in an [ECC \(symmetric\) key pair](key_mgmt_util-genSymKey.md)\. The public key has key handle `262179`\. The private key has key handle `262177`\.   
When you run getKeyInfo on the private key \(`262177`\), it returns the key owner \(3\) and crypto users \(CUs\) 4, with whom the key is shared\.   

```
aws-cloudhsm>getKeyInfo -k 262177
Key Info on server 0(10.0.0.1):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
Key Info on server 1(10.0.0.2):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
```
When you run getKeyInfo on the public key \(`262179`\), it returns only the key owner, user `3`\.   

```
aws-cloudhsm>getKeyInfo -k 262179
Key Info on server 0(10.0.3.10):

        Token/Flash Key,

        Owned by user 3

Key Info on server 1(10.0.3.6):

        Token/Flash Key,

        Owned by user 3
```
To confirm that user 4 can use the public key \(and all public keys on the HSM\), use the `-u` parameter of [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util\.   
The output shows that user 4 can use both the public \(`262179`\) and private \(`262177`\) key in the key pair\. User 4 can also use all other public keys and any private keys that they have created or that have been shared with them\.   

```
Command:  findKey -u 4

Total number of keys present 8

 number of keys matched from start index 0::7
11, 12, 262159, 262161, 262162, 19, 20, 21, 262177, 262179

        Cluster Error Status
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS

        Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS
```

**Example : Get the Quorum Authentication Value \(m\_value\) for a Key**  
This example shows how to get the `m_value` for a key\. The m\_value is the number of users in the quorum who must approve any cryptographic operations that use the key and operations to share the unshare the key\.  
When quorum authentication is enabled on a key, a quorum of users must approve any cryptographic operations that use the key\. To enable quorum authentication and set the quorum size, use the `-m_value` parameter when you create the key\.  
This command uses [genSymKey](key_mgmt_util-genSymKey.md) to create a 256\-bit AES key that is shared with user 4\. It uses the `m_value` parameter to enable quorum authentication and set the quorum size to two users\. The number of users must be large enough to provide the required approvals\.  
The output shows that the command created key 10\.  

```
 Command:  genSymKey -t 31 -s 32 -l aes256m2 -u 4 -m_value 2

        Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

        Symmetric Key Created.  Key Handle: 10

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```
This command uses getKeyInfo in cloudhsm\_mgmt\_util to get information about the users of key `10`\. The output shows that the key is owned by user `3` and shared with user `4`\. It also shows that a quorum of two users must approve every cryptographic operation that uses the key\.  

```
aws-cloudhsm>getKeyInfo 10

Key Info on server 0(10.0.0.1):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
         2 Users need to approve to use/manage this key
Key Info on server 1(10.0.0.2):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
         2 Users need to approve to use/manage this key
```

## Arguments<a name="chmu-getKeyInfo-parameters"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
getKeyInfo -k <key-handle> <output file>
```

**<key\-handle>**  
Specifies the key handle of one key in the HSM\. Enter the key handle of a key that you own or share\. This parameter is required\.   
Required: Yes

**<output file>**  
Writes the output to the specified file, instead of stdout\. If the file exists, the command overwrites it without warning\.  
Required: No  
Default: stdout

## Related Topics<a name="chmu-getKeyInfo-seealso"></a>

+ [getKeyInfo](key_mgmt_util-getKeyInfo.md) in key\_mgmt\_util

+ [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util

+ [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) in cloudhsm\_mgmt\_util

+ [listUsers](cloudhsm_mgmt_util-listUsers.md)

+ [shareKey](cloudhsm_mgmt_util-shareKey.md)