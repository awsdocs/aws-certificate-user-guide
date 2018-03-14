# listUsers<a name="key_mgmt_util-listUsers"></a>

The listUsers command in the key\_mgmt\_util gets the users in the HSMs, along with their user type and other attributes\.

In key\_mgmt\_util, listUsers returns output that represents all HSMs in the cluster, even if they are not consistent\. To get information about the users in each HSM, use the [listUsers](#key_mgmt_util-listUsers) command in cloudhsm\_mgmt\_util\.

The user commands in key\_mgmt\_util, listUsers and getKeyInfo, are read\-only commands that crypto users \(CUs\) have permission to run\. The remaining user management commands are part of cloudhsm\_mgmt\_util\. They are run by crypto officers \(CO\) who have user management permissions\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="listUsers-syntax"></a>

```
listUsers 

listUsers -h
```

## Example<a name="listUsers-examples"></a>

This command lists the users of HSMs in the cluster and their attributes\. You can use the `User ID` attribute to identify users in other commands, such as [findKey](key_mgmt_util-findKey.md), [getAttribute](key_mgmt_util-getAttribute.md), and [getKeyInfo](key_mgmt_util-getKeyInfo.md)\.

```
Command:  listUsers

        Number Of Users found 4

        Index       User ID     User Type       User Name           MofnPubKey    LoginFailureCnt         2FA
        1                1      PCO             admin                     NO               0               NO
        2                2      AU              app_user                  NO               0               NO
        3                3      CU              alice                     YES              0               NO
        4                4      CU              bob                       NO               0               NO
        5                5      CU              trent                     YES              0               NO

        Cfm3ListUsers returned: 0x00 : HSM Return: SUCCESS
```

The output includes the following user attributes:

+ **User ID**: Identifies the user in key\_mgmt\_util and [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) commands\.

+ [User type](hsm-users.md): Determines the operations that the user can perform on the HSM\.

+ **User Name**: Displays the user\-defined friendly name for the user\.

+ **MofnPubKey**: Indicates whether the user has registered a key pair for signing [quorum authentication tokens](quorum-authentication.md)\.

+ **LoginFailureCnt**: 

+ **2FA**: Indicates that the user has enabled multi\-factor authentication\. 

## Parameters<a name="listUsers-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

## Related Topics<a name="listUsers-seealso"></a>

+ [listUsers](#key_mgmt_util-listUsers) in cloudhsm\_mgmt\_util

+ [findKey](key_mgmt_util-findKey.md)

+ [getAttribute](key_mgmt_util-getAttribute.md)

+ [getKeyInfo](key_mgmt_util-getKeyInfo.md)