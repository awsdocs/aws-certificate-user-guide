# Getting Started with AWS CloudHSM: Activate the Cluster<a name="activate-cluster"></a>

When you activate an AWS CloudHSM cluster, the cluster's state changes from initialized to active\. You can then [manage the HSM's users](manage-hsm-users.md) and [use the HSM](use-hsm.md)\.

To activate the cluster, you log in to the HSM with the credentials of the HSM's [precrypto officer \(PRECO\) user](hsm-users.md)\. Then you change the user's default password, which makes the user a crypto officer \(CO\)\.

**To activate a cluster**

1. If you haven't already done so, connect to the client instance that you [created previously](launch-client-instance.md)\. You can do this [using SSH \(Linux or macOS\)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) or [from Windows using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)\.

1. Use the following command to start the AWS CloudHSM [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to enable end\-to\-end encryption\.

   ```
   aws-cloudhsm>enable_e2e
   E2E enabled on server 0(server1)
   ```

1. \(Optional\) Use the listUsers command to display the existing users\.

   ```
   aws-cloudhsm>listUsers
   Users on server 0(server1):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PRECO           admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   ```

1. Use the loginHSM command to log in to the HSM as the [precrypto officer \(PRECO\)](hsm-users.md#preco) user\.

   ```
   aws-cloudhsm>loginHSM PRECO admin password
   loginHSM success on server 0(server1)
   ```

1. Use the changePswd command to change the precrypto officer \(PRECO\) user's password\.

   ```
   aws-cloudhsm>changePswd PRECO admin <NewPassword>
   *************************CAUTION********************************
   This is a CRITICAL operation, should be done on all nodes in the
   cluster. Cav server does NOT synchronize these changes with the
   nodes on which this operation is not executed or failed, please
   ensure this operation is executed on all nodes in the cluster.
   ****************************************************************
   
   Do you want to continue(y/n)?y
   Changing password for admin(PRECO) on 1 nodes
   ```

   We recommend that you write down the new password on a password worksheet\. Do not lose the worksheet\. We recommend that you print a copy of the password worksheet, use it to record your critical HSM passwords, and then store it in a secure place\. We also recommended that you store a copy of this worksheet in secure off\-site storage\.

1. \(Optional\) Use the listUsers command to verify that the user's type changed to \(primary\) [crypto officer \(PCO\)](hsm-users.md#crypto-officer)\.

   ```
   aws-cloudhsm>listUsers
   Users on server 0(server1):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PCO             admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   ```

1. Use the quit command to stop the cloudhsm\_mgmt\_util tool\.

   ```
   aws-cloudhsm>quit
   ```