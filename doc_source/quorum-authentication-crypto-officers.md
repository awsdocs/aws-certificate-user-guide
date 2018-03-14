# Using Quorum Authentication for Crypto Officers<a name="quorum-authentication-crypto-officers"></a>

A [crypto officer \(CO\)](hsm-users.md#crypto-officer) on the HSM can configure quorum authentication for the following operations on the HSM:

+ Creating HSM users

+ Deleting HSM users

+ Changing another HSM user's password

After the HSM is configured for quorum authentication, COs cannot perform HSM user management operations on their own\. The following example shows the output when a CO attempts to create a new user on the HSM\. The command fails with a `RET_MXN_AUTH_FAILED` error, which indicates that quorum authentication failed\.

```
aws-cloudhsm>createUser CU user1 password
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Creating User user1(CU) on 2 nodes
createUser failed: RET_MXN_AUTH_FAILED
creating user on server 0(10.0.2.14) failed

Retry/Ignore/Abort?(R/I/A):A
```

To perform an HSM user management operation, a CO must complete the following tasks:

1. [Get a *quorum token*](#quorum-crypto-officers-get-token)\.

1. [Get approvals \(signatures\) from other COs](#quorum-crypto-officers-get-approval-signatures)\.

1. [Approve the token on the HSM](#quorum-crypto-officers-approve-token)\.

1. [Perform the HSM user management operation](#quorum-crypto-officers-use-token)\.

If you have not yet configured the HSM for quorum authentication for COs, do that now\. For more information, see [First Time Setup for Crypto Officers](quorum-authentication-crypto-officers-first-time-setup.md)\.

## Get a Quorum Token<a name="quorum-crypto-officers-get-token"></a>

First the CO must use the cloudhsm\_mgmt\_util command line tool to request a *quorum token*\.

**To get a quorum token**

1. Use the following command to start the cloudhsm\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to establish end\-to\-end encrypted communication\.

1. Use the loginHSM command to log in to the HSM as a CO\. For more information, see [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in)\.

1. Use the getToken command to get a quorum token\. For more information, see the following example or use the help getToken command\.

**Example – Get a quorum token**  
This example gets a quorum token for the CO with user name officer1 and saves the token to a file named `officer1.token`\. To use the example command, replace these values with your own:  

+ *officer1* – The name of the CO who is getting the token\. This must be the same CO who is logged in to the HSM and is running this command\.

+ *officer1\.token* – The name of the file to use for storing the quorum token\.
In the following command, `3` identifies the *service* for which you can use the token that you are getting\. In this case, the token is for HSM user management operations \(service 3\)\. For more information, see [Set the Quorum Minimum Value on the HSM](quorum-authentication-crypto-officers-first-time-setup.md#quorum-crypto-officers-set-quorum-minimum-value)\.  

```
aws-cloudhsm>getToken 3 officer1 officer1.token
getToken success on server 0(10.0.2.14)
Token:
Id:1
Service:3
Node:1
Key Handle:0
User:officer1
getToken success on server 1(10.0.1.4)
Token:
Id:1
Service:3
Node:0
Key Handle:0
User:officer1
```

## Get Signatures from Approving COs<a name="quorum-crypto-officers-get-approval-signatures"></a>

A CO who has a quorum token must get the token approved by other COs\. To give their approval, the other COs use their signing key to cryptographically sign the token\. They do this outside the HSM\.

There are many different ways to sign the token\. The following example shows how to do it with [OpenSSL](https://www.openssl.org/)\. To use a different signing tool, make sure that the tool uses the CO's private key \(signing key\) to sign a SHA\-256 digest of the token\.

**Example – Get signatures from approving COs**  
In this example, the CO that has the token \(officer1\) needs at least two approvals\. The following example commands show how two COs can use OpenSSL to cryptographically sign the token\.  
In the first command, officer1 signs his or her own token\. To use the following example commands, replace these values with your own:  

+ *officer1\.key* and *officer2\.key* – The name of the file that contains the CO's signing key\.

+ *officer1\.token\.sig1* and *officer1\.token\.sig2* – The name of the file to use for storing the signature\. Make sure to save each signature in a different file\.

+ *officer1\.token* – The name of the file that contains the token that the CO is signing\.

```
$ openssl dgst -sha256 -sign officer1.key -out officer1.token.sig1 officer1.token
Enter pass phrase for officer1.key:
```
In the following command, officer2 signs the same token\.  

```
$ openssl dgst -sha256 -sign officer2.key -out officer1.token.sig2 officer1.token
Enter pass phrase for officer2.key:
```

## Approve the Signed Token on the HSM<a name="quorum-crypto-officers-approve-token"></a>

After a CO gets the minimum number of approvals \(signatures\) from other COs, he or she must approve the signed token on the HSM\.

**To approve the signed token on the HSM**

1. Create a token approval file\. For more information, see the following example\.

1. Use the following command to start the cloudhsm\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to establish end\-to\-end encrypted communication\.

1. Use the loginHSM command to log in to the HSM as a CO\. For more information, see [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in)\.

1. Use the approveToken command to approve the signed token, passing the token approval file\. For more information, see the following example\.

**Example – Create a token approval file and approve the signed token on the HSM**  
The token approval file is a text file in a particular format that the HSM requires\. The file contains information about the token, its approvers, and the approvers' signatures\. The following shows an example token approval file\.  

```
# For "Multi Token File Path", type the path to the file that contains
# the token. You can type the same value for "Token File Path", but
# that's not required. The "Token File Path" line is required in any
# case, regardless of whether you type a value.
Multi Token File Path = officer1.token;
Token File Path = ;

# Total number of approvals
Number of Approvals = 2;

# Approver 1
# Type the approver's type, name, and the path to the file that
# contains the approver's signature.
Approver Type = 2; # 2 for CO, 1 for CU
Approver Name = officer1;
Approval File = officer1.token.sig1;

# Approver 2
# Type the approver's type, name, and the path to the file that
# contains the approver's signature.
Approver Type = 2; # 2 for CO, 1 for CU
Approver Name = officer2;
Approval File = officer1.token.sig2;
```
After creating the token approval file, the CO uses the cloudhsm\_mgmt\_util command line tool to log in to the HSM\. The CO then uses the approveToken command to approve the token, as shown in the following example\. Replace *approval\.txt* with the name of the token approval file\.  

```
aws-cloudhsm>approveToken approval.txt
approveToken success on server 0(10.0.2.14)
approveToken success on server 1(10.0.1.4)
```
When this command succeeds, the HSM has approved the quorum token\. To check the status of a token, use the listTokens command, as shown in the following example\. The command's output shows that the token has the required number of approvals\.  
The token validity time indicates how long the token is guaranteed to persist on the HSM\. Even after the token validity time elapses \(zero seconds\), you can still use the token\.  

```
aws-cloudhsm>listTokens

=====================
    Server 0(10.0.2.14)
=====================
-------- Token - 0 ----------
Token:
Id:1
Service:3
Node:1
Key Handle:0
User:officer1
Token Validity: 506 sec
Required num of approvers : 2
Current num of approvals : 2
Approver-0: officer1
Approver-1: officer2
Num of tokens = 1

=====================
    Server 1(10.0.1.4)
=====================
-------- Token - 0 ----------
Token:
Id:1
Service:3
Node:0
Key Handle:0
User:officer1
Token Validity: 506 sec
Required num of approvers : 2
Current num of approvals : 2
Approver-0: officer1
Approver-1: officer2
Num of tokens = 1

listTokens success
```

## Use the Token for User Management Operations<a name="quorum-crypto-officers-use-token"></a>

After a CO has a token with the required number of approvals, as shown in the previous section, the CO can perform one of the following HSM user management operations:

+ Create an HSM user with the [createUser](cloudhsm_mgmt_util-createUser.md) command

+ Delete an HSM user with the deleteUser command

+ Change a different HSM user's password with the changePswd command

For more information about using these commands, see [Managing HSM Users](manage-hsm-users.md)\.

The CO can use the token for only one operation\. When that operation succeeds, the token is no longer valid\. To do another HSM user management operation, the CO must get a new quorum token, get new signatures from approvers, and approve the new token on the HSM\.

In the following example command, the CO creates a new user on the HSM\.

```
aws-cloudhsm>createUser CU user1 password
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Creating User user1(CU) on 2 nodes
```

After the previous command succeeds, a subsequent listUsers command shows the new user\.

```
aws-cloudhsm>listUsers
Users on server 0(10.0.2.14):
Number of users found:8

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                YES               0               NO
         4              CO              officer2                                YES               0               NO
         5              CO              officer3                                YES               0               NO
         6              CO              officer4                                YES               0               NO
         7              CO              officer5                                YES               0               NO
         8              CU              user1                                    NO               0               NO
Users on server 1(10.0.1.4):
Number of users found:8

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                    NO               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              officer1                                YES               0               NO
         4              CO              officer2                                YES               0               NO
         5              CO              officer3                                YES               0               NO
         6              CO              officer4                                YES               0               NO
         7              CO              officer5                                YES               0               NO
         8              CU              user1                                    NO               0               NO
```

If the CO tries to perform another HSM user management operation, it fails with a quorum authentication error, as shown in the following example\.

```
aws-cloudhsm>deleteUser CU user1
Deleting user user1(CU) on 2 nodes
deleteUser failed: RET_MXN_AUTH_FAILED
deleteUser failed on server 0(10.0.2.14)

Retry/rollBack/Ignore?(R/B/I):I
deleteUser failed: RET_MXN_AUTH_FAILED
deleteUser failed on server 1(10.0.1.4)

Retry/rollBack/Ignore?(R/B/I):I
```

The listTokens command shows that the CO has no approved tokens, as shown in the following example\. To perform another HSM user management operation, the CO must get a new quorum token, get new signatures from approvers, and approve the new token on the HSM\.

```
aws-cloudhsm>listTokens

=====================
    Server 0(10.0.2.14)
=====================
Num of tokens = 0

=====================
    Server 1(10.0.1.4)
=====================
Num of tokens = 0

listTokens success
```