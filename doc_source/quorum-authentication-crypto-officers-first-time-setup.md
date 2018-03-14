# Using Quorum Authentication for Crypto Officers: First Time Setup<a name="quorum-authentication-crypto-officers-first-time-setup"></a>

The following topics describe the steps that you must complete to configure your HSM so that [crypto officers \(COs\)](hsm-users.md#crypto-officer) can use quorum authentication\. You need to do these steps only once when you first configure quorum authentication for COs\. After you complete these steps, see [Using Quorum Authentication for Crypto Officers](quorum-authentication-crypto-officers.md)\.


+ [Prerequisites](#quorum-crypto-officers-prerequisites)
+ [Create and Register a Key for Signing](#quorum-crypto-officers-create-and-register-key)
+ [Set the Quorum Minimum Value on the HSM](#quorum-crypto-officers-set-quorum-minimum-value)

## Prerequisites<a name="quorum-crypto-officers-prerequisites"></a>

To understand this example, you should be familiar with the [cloudhsm\_mgmt\_util command line tool](cloudhsm_mgmt_util.md)\. In this example, the AWS CloudHSM cluster has two HSMs, each with the same COs, as shown in the following output from the listUsers command\. For more information about creating users, see [Managing HSM Users](manage-hsm-users.md)\.

```
aws-cloudhsm>listUsers
Users on server 0(10.0.2.14):
Number of users found:7

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                 NO               0               NO
         4              CO              officer2                                 NO               0               NO
         5              CO              officer3                                 NO               0               NO
         6              CO              officer4                                 NO               0               NO
         7              CO              officer5                                 NO               0               NO
Users on server 1(10.0.1.4):
Number of users found:7

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                 NO               0               NO
         4              CO              officer2                                 NO               0               NO
         5              CO              officer3                                 NO               0               NO
         6              CO              officer4                                 NO               0               NO
         7              CO              officer5                                 NO               0               NO
```

## Create and Register a Key for Signing<a name="quorum-crypto-officers-create-and-register-key"></a>

To use quorum authentication, each CO must create an asymmetric key for signing \(a *signing key*\)\. This is done outside of the HSM\.

There are many different ways to create and protect a personal signing key\. The following example shows how to do it with [OpenSSL](https://www.openssl.org/)\.

**Example – Create a personal signing key with OpenSSL**  
The following example demonstrates how to use OpenSSL to create a 2048\-bit RSA key that is protected by a pass phrase\. To use this example, replace *officer1\.key* with the name of the file where you want to store the key\.  

```
$ openssl genrsa -out officer1.key -aes256 2048
Generating RSA private key, 2048 bit long modulus
.....................................+++
.+++
e is 65537 (0x10001)
Enter pass phrase for officer1.key:
Verifying - Enter pass phrase for officer1.key:
```

Each CO should create his or her own key\.

After creating a key, the CO must register the public part of the key \(the public key\) with the HSM\.

**To register a public key with the HSM**

1. Use the following command to start the cloudhsm\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to establish end\-to\-end encrypted communication\.

1. Use the loginHSM command to log in to the HSM as a CO\. For more information, see [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in)\.

1. Use the registerMofnPubKey command to register the public key\. For more information, see the following example or use the help registerMofnPubKey command\.

**Example – Register a public key with the HSM**  
The following example shows how to use the registerMofnPubKey command in the cloudhsm\_mgmt\_util command line tool to register a CO's public key with the HSM\. To use this command, the CO must be logged in to the HSM\. Replace these values with your own:  

+ *key\_match\_string* – An arbitrary string that is used to match the public and private keys\. You can use any string for this value\. The cloudhsm\_mgmt\_util command line tool encrypts this string with the private key, and then sends the encrypted blob and the plaintext string to the HSM\. The HSM uses the public key to decrypt the encrypted blob, and then compares the decrypted string to the plaintext string\. If the strings match, the HSM registers the public key; otherwise it doesn't\.

+ *officer1* – The user name of the CO who is registering the public key\. This must be the same CO who is logged in to the HSM and is running this command\.

+ *officer1\.key* – The name of the file that contains the CO's key\. This file must contain the complete key \(not just the public part\) because the cloudhsm\_mgmt\_util command line tool uses the private key to encrypt the *key match string*\.
When prompted, type the pass phrase that protects the CO's key\.  

```
aws-cloudhsm>registerMofnPubKey CO key_match_string officer1 officer1.key
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Enter PEM pass phrase:
registerMofnPubKey success on server 0(10.0.2.14)
registerMofnPubKey success on server 1(10.0.1.4)
```

Each CO must register his or her public key with the HSM\. After all COs register their public keys, the output from the listUsers command shows this in the `MofnPubKey` column, as shown in the following example\.

```
aws-cloudhsm>listUsers
Users on server 0(10.0.2.14):
Number of users found:7

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                YES               0               NO
         4              CO              officer2                                YES               0               NO
         5              CO              officer3                                YES               0               NO
         6              CO              officer4                                YES               0               NO
         7              CO              officer5                                YES               0               NO
Users on server 1(10.0.1.4):
Number of users found:7

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                YES               0               NO
         4              CO              officer2                                YES               0               NO
         5              CO              officer3                                YES               0               NO
         6              CO              officer4                                YES               0               NO
         7              CO              officer5                                YES               0               NO
```

## Set the Quorum Minimum Value on the HSM<a name="quorum-crypto-officers-set-quorum-minimum-value"></a>

To use quorum authentication for COs, a CO must log in to the HSM and then set the *quorum minimum value*, also known as the *m value*\. This is the minimum number of CO approvals that are required to perform HSM user management operations\. Any CO on the HSM can set the quorum minimum value, including COs that have not registered a key for signing\. You can change the quorum minimum value at any time; for more information, see [Change the Quorum Value for Crypto Officers](quorum-authentication-crypto-officers-change-minimum-value.md)\.

**To set the quorum minimum value on the HSM**

1. Use the following command to start the cloudhsm\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to establish end\-to\-end encrypted communication\.

1. Use the loginHSM command to log in to the HSM as a CO\. For more information, see [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in)\.

1. Use the setMValue command to set the quorum minimum value\. For more information, see the following example or use the help setMValue command\.

**Example – Set the quorum minimum value on the HSM**  
This example uses a quorum minimum value of two\. You can choose any value from two to twenty, up to the total number of COs on the HSM\. In this example, the HSM has six COs \(the [PCO user](hsm-users.md#crypto-officer) is the same as a CO\), so the maximum possible value is six\.  
To use the following example command, replace the final number \(*2*\) with the preferred quorum minimum value\.  

```
aws-cloudhsm>setMValue 3 2
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Setting M Value(2) for 3 on 2 nodes
```

In the preceding example, the first number \(3\) identifies the *HSM service* whose quorum minimum value you are setting\.

The following table lists the HSM service identifiers along with their names, descriptions, and the commands that are included in the service\.


| Service Identifier | Service Name | Service Description | HSM Commands | 
| --- | --- | --- | --- | 
| 3 | USER\_MGMT | HSM user management |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-authentication-crypto-officers-first-time-setup.html)  | 
| 4 | MISC\_CO | Miscellaneous CO service |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-authentication-crypto-officers-first-time-setup.html)  | 

To get the quorum minimum value for a service, use the getMValue command, as in the following example\.

```
aws-cloudhsm>getMValue 3
MValue of service 3[USER_MGMT] on server 0 : [2]
MValue of service 3[USER_MGMT] on server 1 : [2]
```

The output from the preceding getMValue command shows that the quorum minimum value for HSM user management operations \(service 3\) is now two\.

After you complete these steps, see [Using Quorum Authentication for Crypto Officers](quorum-authentication-crypto-officers.md)\.