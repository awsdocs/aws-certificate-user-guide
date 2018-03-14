# info<a name="cloudhsm_mgmt_util-info"></a>

The info command in cloudhsm\_mgmt\_util gets information about each of the HSMs in the cluster, including the host name, port , IP address and the name and type of the user who is logged in to cloudhsm\_mgmt\_util on the HSM\.

Before you run any cloudhsm\_mgmt\_util command, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), [enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e), and [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) to the HSM\. Be sure that the user type of the account that you use to log in can run the commands you plan to use\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="info-userType"></a>

The following types of users can run this command\.

+ All users\. You do not have to be logged in to run this command\.

## Syntax<a name="info-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
info server <server ID>
```

## Example<a name="info-examples"></a>

This example uses info to get information about an HSM in the cluster\. The command uses 0 to refer to the first HSM in the cluster\. The output shows the IP address, port, and the type and names of the current user\.

```
aws-cloudhsm> info server 0
Id      Name                    Hostname         Port   State           Partition               LoginState
0       10.0.0.1                10.0.0.1        2225    Connected       hsm-udw0tkfg1ab         Logged in as 'testuser(CU)'
```

## Arguments<a name="info-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
info server <server ID>
```

**<server id>**  
Specifies the server ID of the HSM\. The HSMs are assigned ordinal numbers that represent the order in which they are added to the cluster, beginning with 0\. To find the server ID of an HSM, use getHSMInfo\.  
Required: Yes

## Related Topics<a name="info-seealso"></a>

+ [getHSMInfo](cloudhsm_mgmt_util-getHSMInfo.md)

+ loginHSM