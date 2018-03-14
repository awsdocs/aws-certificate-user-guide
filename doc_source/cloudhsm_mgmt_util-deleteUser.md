# deleteUser<a name="cloudhsm_mgmt_util-deleteUser"></a>

The deleteUser command in cloudhsm\_mgmt\_util deletes a user from the HSMs\. Only crypto officers \(COs and PCOs\) can run this command, but any CO user can delete any user of any type from the HSMs\. However, you cannot delete a user who is logged into the AWS CloudHSM client, key\_mgmt\_util, or cloudhsm\_mgmt\_util\.

**Warning**  
When you delete a crypto user \(CU\), all keys that the user owned are deleted, even if the keys were shared with other users\. To make accidental or malicious deletion of users less likely, use [quorum authentication](quorum-authentication-crypto-officers.md)\. 

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="deleteUser-userType"></a>

The following types of users can run this command\.

+ Crypto officers \(CO, PCO\)

## Syntax<a name="deleteUser-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
deleteUser <user-type> <user-name>
```

## Example<a name="deleteUser-examples"></a>

This example deletes a crypto officer \(CO\) from the HSMs in a cluster\. The first command uses listUsers to list all users on the HSMs\.

The output shows that user `3`, `alice`, is a CO on the HSMs\.

```
aws-cloudhsm> listUsers
Users on server 0(10.0.0.1):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO
Users on server 1(10.0.0.2):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO

Users on server 1(10.0.0.3):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO
```

The second command uses the deleteUser command to delete `alice` from the HSMs\. 

The output shows that the command succeeded on all three HSMs in the cluster\.

```
aws-cloudhsm> deleteUser CO alice
Deleting user alice(CO) on 3 nodes
deleteUser success on server 0(10.0.0.1)
deleteUser success on server 0(10.0.0.2)
deleteUser success on server 0(10.0.0.3)
```

The final command uses the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to verify that `alice` is deleted from all three of the HSMs on the cluster\.

```
aws-cloudhsm> listUsers
Users on server 0(10.0.0.1):
Number of users found:2

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
Users on server 1(10.0.0.2):
Number of users found:2

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
Users on server 1(10.0.0.3):
Number of users found:2

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
```

## Arguments<a name="deleteUser-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
deleteUser <user-type> <user-name>
```

**<user\-type>**  
Specifies the type of user\. This parameter is required\.   
When you delete a crypto user \(CU\), all keys that the user owned are deleted, even if the keys were shared with other users\. To make accidental or malicious deletion of users less likely, use [quorum authentication](quorum-authentication-crypto-officers.md)\. 
Valid values are **CO**, **CU**, **AU**, **PCO**, and **PRECO**\.   
To get the user type, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\. For detailed information about the user types on an HSM, see [HSM Users](hsm-users.md)\.  
Required: Yes

**<user\-name>**  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In cloudhsm\_mgmt\_util commands, the user type and password are case\-sensitive, but the user name is not\.  
Required: Yes

## Related Topics<a name="deleteUser-seealso"></a>

+ [listUsers](cloudhsm_mgmt_util-listUsers.md)

+ [createUser](cloudhsm_mgmt_util-createUser.md)

+ syncUser

+ [changePswd](cloudhsm_mgmt_util-changePswd.md)