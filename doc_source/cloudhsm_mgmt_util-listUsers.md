# listUsers<a name="cloudhsm_mgmt_util-listUsers"></a>

The listUsers command in the cloudhsm\_mgmt\_util gets the users in each of the HSMs, along with their user type and other attributes\. All types of users can run this command\. You do not even need to be logged in to cloudhsm\_mgmt\_util to run this command\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="listUsers-userType"></a>

The following types of users can run this command\.

+ All users\. You do not need to be logged in to run this command\.

## Syntax<a name="chmu-listUsers-syntax"></a>

This command has no parameters\.

```
listUsers
```

## Example<a name="chmu-listUsers-examples"></a>

This command lists the users on each of the HSMs in the cluster and displays their attributes\. You can use the `User ID` attribute to identify users in other commands, such as deleteUser, changePswd, and findAllKeys\.

The output shows that one of the users, officer2, exists only on the first HSM\. To add that user to other HSMs, use syncUser\.

```
aws-cloudhsm> listUsers
Users on server 0(10.0.0.1):
Number of users found:6

    User Id             User Type       User Name            MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                     YES               0               NO
         2              AU              app_user                   NO               0               NO
         3              CU              crypto_user1               NO               0               NO
         4              CU              crypto_user2               NO               0               NO
         5              CO              officer1                  YES               0               NO
         6              CO              officer2                   NO               0               NO
Users on server 1(10.0.0.2):
Number of users found:5

    User Id             User Type       User Name            MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                     YES               0               NO
         2              AU              app_user                   NO               0               NO
         3              CU              crypto_user1               NO               0               NO
         4              CU              crypto_user2               NO               0               NO
         5              CO              officer1                  YES               0               NO
```

The output includes the following user attributes:

+ **User ID**: Identifies the user in key\_mgmt\_util and [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) commands\.

+ [User type](hsm-users.md): Determines the operations that the user can perform on the HSM\.

+ **User Name**: Displays the user\-defined friendly name for the user\.

+ **MofnPubKey**: Indicates whether the user has registered a key pair for signing [quorum authentication tokens](quorum-authentication.md)\.

+ **LoginFailureCnt**: 

+ **2FA**: Indicates that the user has enabled multi\-factor authentication\. 

## Related Topics<a name="chmu-listUsers-seealso"></a>

+ [listUsers](key_mgmt_util-listUsers.md) in key\_mgmt\_util

+ [createUser](cloudhsm_mgmt_util-createUser.md)

+ [deleteUser](cloudhsm_mgmt_util-deleteUser.md)

+ syncUser

+ [changePswd](cloudhsm_mgmt_util-changePswd.md)