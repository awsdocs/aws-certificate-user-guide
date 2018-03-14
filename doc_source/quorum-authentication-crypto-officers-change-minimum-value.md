# Change the Quorum Minimum Value for Crypto Officers<a name="quorum-authentication-crypto-officers-change-minimum-value"></a>

After you [set the quorum minimum value](quorum-authentication-crypto-officers-first-time-setup.md#quorum-crypto-officers-set-quorum-minimum-value) so that [crypto officers \(COs\)](hsm-users.md#crypto-officer) can use quorum authentication, you might want to change the quorum minimum value\. The HSM allows you to change the quorum minimum value only when the number of approvers is the same or higher than the current quorum minimum value\. For example, if the quorum minimum value is two, at least two COs must approve to change the quorum minimum value\.

To get quorum approval to change the quorum minimum value, you need a *quorum token* for the setMValue command \(service 4\)\. To get a quorum token for the setMValue command \(service 4\), the quorum minimum value for service 4 must be higher than one\. This means that before you can change the quorum minimum value for COs \(service 3\), you might need to change the quorum minimum value for service 4\.

The following table lists the HSM service identifiers along with their names, descriptions, and the commands that are included in the service\.


| Service Identifier | Service Name | Service Description | HSM Commands | 
| --- | --- | --- | --- | 
| 3 | USER\_MGMT | HSM user management |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-authentication-crypto-officers-change-minimum-value.html)  | 
| 4 | MISC\_CO | Miscellaneous CO service |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/quorum-authentication-crypto-officers-change-minimum-value.html)  | 

**To change the quorum minimum value for crypto officers**

1. Use the following command to start the cloudhsm\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Use the enable\_e2e command to establish end\-to\-end encrypted communication\.

1. Use the loginHSM command to log in to the HSM as a CO\. For more information, see [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in)\.

1. Use the getMValue command to get the quorum minimum value for service 3\. For more information, see the following example\.

1. Use the getMValue command to get the quorum minimum value for service 4\. For more information, see the following example\.

1. If the quorum minimum value for service 4 is lower than the value for service 3, use the setMValue command to change the value for service 4\. Change the value for service 4 to one that is the same or higher than the value for service 3\. For more information, see the following example\.

1. [Get a *quorum token*](quorum-authentication-crypto-officers.md#quorum-crypto-officers-get-token), taking care to specify service 4 as the service for which you can use the token\.

1. [Get approvals \(signatures\) from other COs](quorum-authentication-crypto-officers.md#quorum-crypto-officers-get-approval-signatures)\.

1. [Approve the token on the HSM](quorum-authentication-crypto-officers.md#quorum-crypto-officers-approve-token)\.

1. Use the setMValue command to change quorum minimum value for service 3 \(user management operations performed by COs\)\.

**Example – Get quorum minimum values and change the value for service 4**  
The following example command shows that the quorum minimum value for service 3 is currently two\.  

```
aws-cloudhsm>getMValue 3
MValue of service 3[USER_MGMT] on server 0 : [2]
MValue of service 3[USER_MGMT] on server 1 : [2]
```
The following example command shows that the quorum minimum value for service 4 is currently one\.  

```
aws-cloudhsm>getMValue 4
MValue of service 4[MISC_CO] on server 0 : [1]
MValue of service 4[MISC_CO] on server 1 : [1]
```
To change the quorum minimum value for service 4, use the setMValue command, setting a value that is the same or higher than the value for service 3\. The following example sets the quorum minimum value for service 4 to two \(2\), the same value that is set for service 3\.  

```
aws-cloudhsm>setMValue 4 2
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Setting M Value(2) for 4 on 2 nodes
```
The following commands show that the quorum minimum value is now two for service 3 and service 4\.  

```
aws-cloudhsm>getMValue 3
MValue of service 3[USER_MGMT] on server 0 : [2]
MValue of service 3[USER_MGMT] on server 1 : [2]
```

```
aws-cloudhsm>getMValue 4
MValue of service 4[MISC_CO] on server 0 : [2]
MValue of service 4[MISC_CO] on server 1 : [2]
```