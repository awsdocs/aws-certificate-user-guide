# findAllKeys<a name="cloudhsm_mgmt_util-findAllKeys"></a>

The findAllKeys command in cloudhsm\_mgmt\_util gets the keys that a specified crypto user \(CU\) owns or shares\. It also returns a hash of the user data on each of the HSMs\. You can use the hash to determine at a glance whether the users, key ownership, and key sharing data are the same on all HSMs in the cluster\.

findAllKeys returns public keys only when the specified CU owns the key, even though all CUs on the HSM can use any public key\. This behavior is different from [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util, which returns public keys for all CU users\.

Only crypto officers \(COs and PCOs\) and appliance users \(AUs\) can run this command\. Crypto users \(CUs\) can run [listUsers](cloudhsm_mgmt_util-listUsers.md) to find all users , [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util to find the keys that they can use and [getKeyInfo](key_mgmt_util-getKeyInfo.md) in key\_mgmt\_util to find the owner and shared users of a particular key they own or share\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="findAllKeys-userType"></a>

The following types of users can run this command\.

+ Crypto officers \(CO, PCO\)

+ Appliance users \(AU\)

## Syntax<a name="findAllKeys-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
findAllKeys<user id> <key hash (0/1)> [<output file>]
```

## Examples<a name="findAllKeys-examples"></a>

These examples show how to use findAllKeys to find all keys for a user and get a hash of key user information on each of the HSMs\.

**Example : Find the Keys for a CU**  
This example uses findAllKeys to find the keys in the HSMs that user 4 owns and shares\. The command uses a value of `0` for the second argument to suppress the hash value\. Because it omits the optional file name, the command writes to stdout\.  
The output shows that user 4 can use 6 keys: 8, 9, 17, 262162, 19, and 31\. The output uses an `(s)` to indicate that keys 8, 9, and 262162 are explicitly shared, although it does not indicate whether user 4 owns or shares them\. The keys that are not marked with `(s)` include symmetric and private keys that the user 4 owns and does not share, and public keys that are available to all crypto users\.   

```
aws-cloudhsm> findAllKey 4 0
Keys on server 0(10.0.0.1):
Number of keys found 6
number of keys matched from start index 0::6
8(s),9(s),17,262162(s),19,31
findAllKeys success on server 0(10.0.0.1)

Keys on server 1(10.0.0.2):
Number of keys found 6
number of keys matched from start index 0::6
8(s),9(s),17,262162(s),19,31
findAllKeys success on server 1(10.0.0.2)

Keys on server 1(10.0.0.3):
Number of keys found 6
number of keys matched from start index 0::6
8(s),9(s),17,262162(s),19,31
findAllKeys success on server 1(10.0.0.3)
```

**Example : Verify That User Data is Synchronized**  
This example uses findAllKeys to verify that all of the HSMs in the cluster contain the same users, key ownership, and key sharing values\. To do this, it gets a hash of the key user data on each HSM and compares the hash values\.  
To get the key hash, the command uses a value of `1` in the second argument\. The optional file name is omitted, so the command writes the key hash to stdout\.   
The example specifies user `6`, but the hash value will be the same for any user that owns or shares any of the keys on the HSMs\. If the specified user does not own or share any keys, such as a CO, the command does not return a hash value\.   
The output shows that the key hash is identical both of the HSMs in the cluster\. If one of the HSM had different users, different key owners, or different shared users, the key hash values would not be equal\.  

```
aws-cloudhsm> findAllKeys 6 1
Keys on server 0(10.0.0.1):
Number of keys found 3
number of keys matched from start index 0::3
8(s),9(s),11,17(s)
Key Hash:
55655676c95547fd4e82189a072ee1100eccfca6f10509077a0d6936a976bd49

findAllKeys success on server 0(10.0.0.1)
Keys on server 1(10.0.0.2):
Number of keys found 3
number of keys matched from start index 0::3
8(s),9(s),11,17(s)
Key Hash:
55655676c95547fd4e82189a072ee1100eccfca6f10509077a0d6936a976bd49

findAllKeys success on server 1(10.0.0.2)
```
This command demonstrates that the hash value represents the user data for all keys on the HSM\. The command uses the findAllKeys for user 3\. Unlike user 6, who owns or shares just 3 keys, user 3 own or shares 17 keys, but the key hash value is the same\.  

```
aws-cloudhsm> findAllKeys 3 1
Keys on server 0(10.0.0.1):
Number of keys found 17
number of keys matched from start index 0::17
6,7,8(s),11,12,14,262159,262160,17(s),262162(s),19(s),20,21,262177,262179,262180,262181
Key Hash:
55655676c95547fd4e82189a072ee1100eccfca6f10509077a0d6936a976bd49

findAllKeys success on server 0(10.0.0.1)
Keys on server 1(10.0.0.2):
Number of keys found 17
number of keys matched from start index 0::17
6,7,8(s),11,12,14,262159,262160,17(s),262162(s),19(s),20,21,262177,262179,262180,262181
Key Hash:
55655676c95547fd4e82189a072ee1100eccfca6f10509077a0d6936a976bd49

findAllKeys success on server 1(10.0.0.2)
```

## Arguments<a name="findAllKeys-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
findAllKeys <user id> <key hash (0/1)> [<output file>]
```

**<user id>**  
Gets all keys that the specified user owns or shares\. Enter the user ID of a user on the HSMs\. To find the user IDs of all users, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\.  
All users ID are valid, but findAllKeys returns keys only for crypto users \(CUs\)\.  
Required: Yes

**<key hash>**  
Includes \(`1`\) or excludes \(`0`\) a hash of the user ownership and sharing data for all keys on each HSM\.  
When the `user id` argument represents a user who owns or shares keys, the key hash is populated\. The key hash value is identical for all users who own or share keys on the HSM, even though they own and share different keys\. However, when the `user id` represents a user who does not own or share any keys, such as a CO, the hash value is not populated\.  
Required: Yes

**<output file>**  
Writes the output to the specified file\.   
Required: No  
Default: Stdout

## Related Topics<a name="findAllKeys-seealso"></a>

+ [changePswd](cloudhsm_mgmt_util-changePswd.md)

+ [deleteUser](cloudhsm_mgmt_util-deleteUser.md)

+ [listUsers](cloudhsm_mgmt_util-listUsers.md)

+ syncUser

+ [findKey](key_mgmt_util-findKey.md) in key\_mgmt\_util

+ [getKeyInfo](key_mgmt_util-getKeyInfo.md) in key\_mgmt\_util