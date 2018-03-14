# cloudhsm\_mgmt\_util Command Reference<a name="cloudhsm_mgmt_util-reference"></a>

The **cloudhsm\_mgmt\_util** command line tool helps Crypto Officers \(PCOs and COs\) manage users in the HSMs\. It also includes commands that allow Crypto Users \(CUs\) to share keys, and get and set key attributes\. These commands complement the primary key management commands in the [key\_mgmt\_util](key_mgmt_util.md) command line tool\. 

For a quick start, see [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md)\. 

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

To list all cloudhsm\_mgmt\_util commands, type:

```
aws-cloudhsm> help
```

To get the syntax for a cloudhsm\_mgmt\_util command, type:

```
aws-cloudhsm> help <command-name>
```

To run a command, type the command name, or enough of the name to distinguish it from the names of other cloudhsm\_mgmt\_util commands\. 

For example, to get a list of users on the HSMs, type listUsers or listU\.

```
aws-cloudhsm> listUsers
```

To end your cloudhsm\_mgmt\_util session, type:

```
aws-cloudhsm> quit
```

For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

The following topics describe commands in cloudhsm\_mgmt\_util\. 

**Note**  
Some commands in key\_mgmt\_util and cloudhsm\_mgmt\_util have the same names\. However, the commands typically have different syntax, different output, and slightly different functionality\.


| Command | Description | User Type | 
| --- | --- | --- | 
| [changePswd](cloudhsm_mgmt_util-changePswd.md) | Changes the passwords of users on the HSMs\. Any user can change their own password\. COs can change anyone's password\. | CO | 
| [createUser](cloudhsm_mgmt_util-createUser.md) | Creates users of all types on the HSMs\. | CO | 
| [deleteUser](cloudhsm_mgmt_util-deleteUser.md) | Deletes users of all types from the HSMs\. | CO | 
| [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) | Gets the keys that a user owns or shares\. Also gets a hash of the key ownership and sharing data for all keys on each HSM\. | CO, AU | 
| [getAttribute](cloudhsm_mgmt_util-getAttribute.md) | Gets an attribute value for an AWS CloudHSM key and writes it to a file or stdout\. | CU | 
| [getHSMInfo](cloudhsm_mgmt_util-getHSMInfo.md) | Gets information about the hardware on which an HSM is running\. | All\. Login is not required\. | 
| [getKeyInfo](cloudhsm_mgmt_util-getHSMInfo.md) | Gets owners, shared users, and the quorum authentication status of a key\. | All\. Login is not required\. | 
| [info](cloudhsm_mgmt_util-info.md) | Gets information about an HSM, including the IP address, host name, port, and current user\. | All\. Login is not required\. | 
| [listUsers](cloudhsm_mgmt_util-listUsers.md) | Gets the users in each of the HSMs, their user type and ID, and other attributes\. | All\. Login is not required\. | 
| [setAttribute](cloudhsm_mgmt_util-setAttribute.md) | Changes the values of the label, encrypt, decrypt, wrap, and unwrap attributes of an existing key\. | CU | 
| [shareKey](cloudhsm_mgmt_util-shareKey.md) | Shares an existing key with other users\. | CU | 