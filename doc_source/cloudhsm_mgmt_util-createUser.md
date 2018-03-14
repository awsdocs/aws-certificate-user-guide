# createUser<a name="cloudhsm_mgmt_util-createUser"></a>

The createUser command in cloudhsm\_mgmt\_util creates a user on the HSMs\. Only crypto officers \(COs and PCOs\) can run this command\. When you create a user, you specify the user type \(CO or CU\), a user name, and a password\. When the command succeeds, it creates the user in all HSMs in the cluster\. 

However, if your HSM configuration is inaccurate, the user might not be created on all HSMs\. To add the user to any HSMs in which it is missing, use syncUser or createUser commands only on the HSMs that are missing that user\. To prevent configuration errors, run the configure tool with the `-m` option\. 

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="createUser-userType"></a>

The following types of users can run this command\.

+ Crypto officers \(CO, PCO\)

## Syntax<a name="createUser-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto officer \(CO, PCO\)

```
createUser <user-type> <user-name> <password> [1FA | 2FA]
```

## Examples<a name="createUser-examples"></a>

These examples show how to use createUser to create new users in your HSMs\.

**Example : Create a Crypto Officer**  
This example creates a crypto officer \(CO\) on the HSMs in a cluster\. The first command uses loginHSM to log in to the HSM as a crypto officer\.  

```
aws-cloudhsm> loginHSM CO admin 735782961

loginHSM success on server 0(10.0.0.1)
loginHSM success on server 1(10.0.0.2)
loginHSM success on server 1(10.0.0.3)
```
The second command uses the createUser command to create `alice`, a new crypto officer on the HSM\.  
The caution message explains that the command creates users on all of the HSMs in the cluster\. But, if the command fails on any HSMs, the user will not exist on those HSMs\. To continue, type `y`\.  
The output shows that the new user was created on all three HSMs in the cluster\.  

```
aws-cloudhsm> createUser CO alice 391019314

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?Invalid option, please type 'y' or 'n'

Do you want to continue(y/n)?y
Creating User alice(CO) on 3 nodes
```
When the command completes, `alice` has the same permissions on the HSM as the `admin` CO user, including changing the password of any user on the HSMs\.  
The final command uses the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to verify that `alice` exists on all three HSMs on the cluster\. The output also shows that `alice` is assigned user ID `3`\.`.` You use the user ID to identify `alice` in other commands, such as [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md)\.  

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

**Example : Create a Crypto User**  
This example creates a crypto user \(CU\), `bob`, on the HSM\. Crypto users can create and manage keys, but they cannot manage users\.   
After you type `y` to respond to the caution message, the output shows that `bob` was created on all three HSMs in the cluster\. The new CU can log in to the HSM to create and manage keys\.  
The command used a password value of `defaultPassword`\. Later, `bob` or any CO can use the [changePswd](cloudhsm_mgmt_util-changePswd.md) command to change his password\.  

```
aws-cloudhsm> createUser CU bob defaultPassword

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?Invalid option, please type 'y' or 'n'

Do you want to continue(y/n)?y
Creating User bob(CU) on 3 nodes
```

## Arguments<a name="createUser-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
createUser <user-type> <user-name> <password> [ 1FA | 2FA ]
```

**<user\-type>**  
Specifies the type of user\. This parameter is required\.   
For detailed information about the user types on an HSM, see [HSM Users](hsm-users.md)\.  
Valid values:  

+ **CO**: Crypto officers can manage users, but they cannot manage keys\. 

+ **CU**: Crypto users can create an manage keys and use keys in cryptographic operations\.

+ **AU**: Appliance users can clone and synchronize operations\. One AU is created for you on each HSM that you install\.
PCO, PRECO, and preCO are also valid values, but they are rarely used\. A PCO is functionally identical to a CO user\. A PRECO user is a temporary type that is created automatically on each HSM\. The PRECO is converted to a PCO when you assign a password during [HSM activation](activate-cluster.md)\.  
Required: Yes

**<user\-name>**  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In cloudhsm\_mgmt\_util commands, the user type and password are case\-sensitive, but the user name is not\.  
Required: Yes

**<password>**  
Specifies a password for the user\. Enter a string of 7 to 32 characters\. This value is case\-sensitive\. The password appears in plaintext when you type it\.   
To change a user password, use changePswd\. Any HSM user can change their own password, but CO users can change the password of any user \(of any type\) on the HSMs\.  
Required: Yes

**1FA | 2FA**  
Enables or disables dual\-factor authentication for the new user\. Enter `1FA` or `2FA`\.  
This parameter is valid only when the cluster has been configured for dual\-factor authentication\.  
Required: No  
Default: 1FA: Dual factor authentication is not enabled\.

## Related Topics<a name="createUser-seealso"></a>

+ [listUsers](cloudhsm_mgmt_util-listUsers.md)

+ [deleteUser](cloudhsm_mgmt_util-deleteUser.md)

+ syncUser

+ [changePswd](cloudhsm_mgmt_util-changePswd.md)