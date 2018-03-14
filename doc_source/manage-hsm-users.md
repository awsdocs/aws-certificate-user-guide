# Managing HSM Users in AWS CloudHSM<a name="manage-hsm-users"></a>

To manage users on the HSMs in your AWS CloudHSM cluster, use the AWS CloudHSM command line tool known as cloudhsm\_mgmt\_util\. Before you can manage users, you must start cloudhsm\_mgmt\_util, enable end\-to\-end encryption, and log in to the HSMs\. For more information, see [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\.

To manage HSM users, log in to the HSM with the user name and password of a [cryptographic officer](hsm-users.md#crypto-officer) \(CO\)\. Only COs can manage other users\. The HSM contains a default CO named admin\. You set this user's password when you [activated the cluster](activate-cluster.md)\.


+ [Create Users](#create-user)
+ [List Users](#list-users)
+ [Change a User's Password](#change-user-password)
+ [Delete Users](#delete-user)

## Create Users<a name="create-user"></a>

Use the [createUser](cloudhsm_mgmt_util-createUser.md) command to create a user on the HSM\. The following examples create new CO and CU users, respectively\. For information about user types, see [HSM Users](hsm-users.md)\. 

```
aws-cloudhsm>createUser CO example_officer <password>
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Creating User example_officer(CO) on 3 nodes
```

```
aws-cloudhsm>createUser CU example_user <password>
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Creating User example_user(CU) on 3 nodes
```

The following shows the syntax for the [createUser](cloudhsm_mgmt_util-createUser.md) command\. User types and passwords are case\-sensitive in cloudhsm\_mgmt\_util commands, but user names are not\.

```
aws-cloudhsm>createUser <user type> <user name> <password>
```

## List Users<a name="list-users"></a>

Use the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to list the users on each HSM in the cluster\. All [HSM user types](hsm-users.md) can use this command; it's not restricted to COs\.

The PCO is the first \("primary"\) CO created on each HSM\. It has the same permissions on the HSM as any other CO\.

```
aws-cloudhsm>listUsers
Users on server 0(10.0.2.9):
Number of users found:4

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              example_officer                          NO               0               NO
         4              CU              example_user                             NO               0               NO
Users on server 1(10.0.3.11):
Number of users found:4

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              example_officer                          NO               0               NO
         4              CU              example_user                             NO               0               NO
Users on server 2(10.0.1.12):
Number of users found:4

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              example_officer                          NO               0               NO
         4              CU              example_user                             NO               0               NO
```

## Change a User's Password<a name="change-user-password"></a>

Use the changePswd command to change the password for the any user\. All [HSM user types](hsm-users.md) can issue this command, but only COs can change the password for other users\. Crypto users \(CUs\) and appliance users \(AUs\) can change only their own password\. The following examples change the password for the CO and CU users that were created in the [Create Users](#create-user) examples\.

```
aws-cloudhsm>changePswd CO example_officer <new password>
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Changing password for example_officer(CO) on 3 nodes
```

```
aws-cloudhsm>changePswd CU example_user <new password>
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Changing password for example_user(CU) on 3 nodes
```

The following shows the syntax for the changePswd command\. User types and passwords are case\-sensitive, but user names are not\.

```
aws-cloudhsm>changePswd <user type> <user name> <new password>
```

**Warning**  
The CO cannot change the password for a user \(CO or CU\) who is currently logged in\.

## Delete Users<a name="delete-user"></a>

Use the deleteUser command to delete a user\. The following examples delete the CO and CU users that were created in the [Create Users](#create-user) examples\.

```
aws-cloudhsm>deleteUser CO example_officer
Deleting user example_officer(CO) on 3 nodes
deleteUser success on server 0(10.0.2.9)
deleteUser success on server 1(10.0.3.11)
deleteUser success on server 2(10.0.1.12)
```

```
aws-cloudhsm>deleteUser CU example_user
Deleting user example_user(CU) on 3 nodes
deleteUser success on server 0(10.0.2.9)
deleteUser success on server 1(10.0.3.11)
deleteUser success on server 2(10.0.1.12)
```

The following shows the syntax for the deleteUser command\.

```
aws-cloudhsm>deleteUser <user type> <user name>
```

**Warning**  
Deleting a CU user will orphan all of the keys owned by that CU and make them unusable\. You will not receive any warning that the user you are about to delete still owns keys in the cluster\. 