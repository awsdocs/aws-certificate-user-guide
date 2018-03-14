# shareKey<a name="cloudhsm_mgmt_util-shareKey"></a>

The shareKey command in cloudhsm\_mgmt\_util shares and unshares keys that you own with other crypto users\. Only the key owner can share and unshare a key\. You can also share a key when you create it\.

Users who share the key can use the key in cryptographic operations, but they cannot delete, export, share, or unshare the key, or change its attributes\. When quorum authentication is enabled on a key, the quorum must approve any operations that share or unshare the key\. 

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="shareKey-userType"></a>

The following types of users can run this command\.

+ Crypto users \(CU\)

## Syntax<a name="shareKey-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto user \(CU\)

```
shareKey <key handle> <user id> <(share/unshare key?) 1/0>
```

## Example<a name="shareKey-examples"></a>

The following examples show how to use shareKey to share and unshare keys that you own with other crypto users\.

**Example : Share a Key**  
This example uses shareKey to share an [ECC private key](key_mgmt_util-genSymKey.md) that the current user owns with another crypto user on the HSMs\. Public keys are available to all users of the HSM, so you cannot share or unshare them\.  
The first command uses [getKeyInfo](cloudhsm_mgmt_util-getKeyInfo.md) to get the user information for key `262177`, an ECC private key on the HSMs\.   
The output shows that key `262177` is owned by user 3, but is not shared\.  

```
aws-cloudhsm>getKeyInfo 262177

Key Info on server 0(10.0.3.10):

        Token/Flash Key,

        Owned by user 3

Key Info on server 1(10.0.3.6):

        Token/Flash Key,

        Owned by user 3
```
This example uses shareKey to share key `262177` with user `4`, another crypto user on the HSMs\. The final argument uses a value of `1` to indicate a share operation\.  
The output shows that the operation succeeded on both HSMs in the cluster\.  

```
aws-cloudhsm>shareKey 262177 4 1
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
shareKey success on server 0(10.0.3.10)
shareKey success on server 1(10.0.3.6)
```
To verify that the operation succeeded, the example repeats the first getKeyInfo command\.  
The output shows that key `262177` is now shared with user `4`\.  

```
aws-cloudhsm>getKeyInfo 262177

Key Info on server 0(10.0.3.10):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
Key Info on server 1(10.0.3.6):

        Token/Flash Key,

        Owned by user 3

        also, shared to following 1 user(s):

                 4
```

**Example : Unshare a Key**  
This example unshares a symmetric key, that is, it removes a crypto user from the list of shared users for the key\.   
This command uses shareKey to remove user `4` from the list of shared users for key `6`\. The final argument uses a value of `0` to indicate an unshare operation\.  
The output shows that the command succeeded on both HSMs\. As a result, user `4` can no longer use key `6` in cryptographic operations\.  

```
aws-cloudhsm>shareKey 6 4 0
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
shareKey success on server 0(10.0.3.10)
shareKey success on server 1(10.0.3.6)
```

## Arguments<a name="shareKey-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
shareKey <key handle> <user id> <(share/unshare key?) 1/0>
```

**<key\-handle>**  
Specifies the key handle of a key that you own\. You can specify only one key in each command\. To get the key handle of a key, use [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util\. To verify that you own a key, use [getKeyInfo](cloudhsm_mgmt_util-getKeyInfo.md)\.  
Required: Yes

**<user id>**  
Specifies the user ID the crypto user \(CU\) with whom you are sharing or unsharing the key\. To find the user ID of a user, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\.  
Required: Yes

**<share 1 or unshare 0>**  
To share the key with the specified user, type `1`\. To unshare the key, that is, to remove the specified user from the list of shared users for the key, type `0`\.  
Required: Yes

## Related Topics<a name="shareKey-seealso"></a>

+ [getKeyInfo](cloudhsm_mgmt_util-getKeyInfo.md)