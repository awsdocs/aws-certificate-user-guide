# Getting Started with cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-getting-started"></a>

AWS CloudHSM includes two command line tools with the [AWS CloudHSM client software](install-and-configure-client.md#install-client)\. The [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-reference.md) tool includes commands to manage HSM users\. The [key\_mgmt\_util](key_mgmt_util-reference.md)tool includes commands to manage keys\. To get started with the cloudhsm\_mgmt\_util command line tool, see the following topics\. 


+ [Prepare to run cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-setup)
+ [Basic Usage of cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-basics)

## Prepare to run cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-setup"></a>

Complete the following steps before you use cloudhsm\_mgmt\_util\. You need to do these steps the first time you use cloudhsm\_mgmt\_util and after you add or remove HSMs in your cluster\. 

In these steps, you will update the HSM list in the configuration files that the AWS CloudHSM client and command line tools use\. Keeping these files updated helps AWS CloudHSM to synchronize data and maintain consistency across all HSMs in the cluster\. 


+ [Step 1: Stop the AWS CloudHSM Client](#cloudhsm_mgmt_util-stop-cloudhsm-client)
+ [Step 2: Update the AWS CloudHSM Configuration Files](#cloudhsm_mgmt_util-config-a)
+ [Step 3: Start the AWS CloudHSM Client](#cloudhsm_mgmt_util-start-cloudhsm-client)
+ [Step 4: Update the cloudhsm\_mgmt\_util Configuration File](#cloudhsm_mgmt_util-update-configuration)

### Step 1: Stop the AWS CloudHSM Client<a name="cloudhsm_mgmt_util-stop-cloudhsm-client"></a>

Before you update the configuration files that the AWS CloudHSM and command line tools use, stop the AWS CloudHSM client\. If the client is already stopped, running the stop command has no harmful effect\. 

```
$ sudo stop cloudhsm-client
```

### Step 2: Update the AWS CloudHSM Configuration Files<a name="cloudhsm_mgmt_util-config-a"></a>

This step uses the `-a` parameter of the [Configure tool](configure-tool.md) which adds the elastic network interface \(ENI\) IP address of one of the HSMs in the cluster to the configuration files that the AWS CloudHSM client and command line tools use\. 

```
$  sudo /opt/cloudhsm/bin/configure -a <ENI IP>
```

To get the ENI IP address of an HSM in your cluster, you can use the [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation, the [describe\-clusters](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cli-guide.htmldescribe-clusters.html) command, or the [Get\-HSM2Cluster](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\. Enter only one ENI IP address, no matter how many HSMs you have in your cluster\. It does not matter which HSM ENI IP address you select\. 

### Step 3: Start the AWS CloudHSM Client<a name="cloudhsm_mgmt_util-start-cloudhsm-client"></a>

Next, start or restart the AWS CloudHSM client\. When the AWS CloudHSM client starts, it uses the ENI IP address in its configuration file to query the cluster\. Then, it adds the ENI IP addresses of all HSMs in the cluster to the cluster information file\. 

```
$ sudo start cloudhsm-client
```

### Step 4: Update the cloudhsm\_mgmt\_util Configuration File<a name="cloudhsm_mgmt_util-update-configuration"></a>

The final step uses the `-m` parameter of the [Configuration tool](configure-tool.md) to copy the updated ENI IP addresses from the cluster information file to the configuration file that cloudhsm\_mgmt\_util uses\. If you skip this step, you might run into synchronization problems, such as [inconsistent user data](troubleshooting-keep-hsm-users-in-sync.md) in your cluster's HSMs\. 

```
$ sudo /opt/cloudhsm/bin/configure -m
```

When this step completes, you are ready to start cloudhsm\_mgmt\_util\. If you add or delete HSMs at any time, be sure to repeat this procedure before using cloudhsm\_mgmt\_util\. 

## Basic Usage of cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-basics"></a>

See the following topics for the basic usage of the cloudhsm\_mgmt\_util tool\.

**Note**  
The cloudhsm\_mgmt\_util tool doesn't support auto\-completing commands with the **Tab** key\. Don't use the **Tab** key with cloudhsm\_mgmt\_util, because that can make the tool unresponsive\.


+ [Start cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-start)
+ [Enable End\-to\-End Encryption](#cloudhsm_mgmt_util-enable_e2e)
+ [Log in to the HSMs](#cloudhsm_mgmt_util-log-in)
+ [Log Out from the HSMs](#cloudhsm_mgmt_util-log-out)
+ [Stop cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-stop)

### Start cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-start"></a>

Use the following command to start cloudhsm\_mgmt\_util\.

```
$ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg

Connecting to the server(s), it may take time
depending on the server(s) load, please wait...

Connecting to server '10.0.2.9': hostname '10.0.2.9', port 2225...
Connected to server '10.0.2.9': hostname '10.0.2.9', port 2225.

Connecting to server '10.0.3.11': hostname '10.0.3.11', port 2225...
Connected to server '10.0.3.11': hostname '10.0.3.11', port 2225.

Connecting to server '10.0.1.12': hostname '10.0.1.12', port 2225...
Connected to server '10.0.1.12': hostname '10.0.1.12', port 2225.
```

The prompt changes to aws\-cloudhsm> when cloudhsm\_mgmt\_util is running\.

### Enable End\-to\-End Encryption<a name="cloudhsm_mgmt_util-enable_e2e"></a>

Use the enable\_e2e command to establish end\-to\-end encrypted communication between cloudhsm\_mgmt\_util and the HSMs in your cluster\. You should enable end\-to\-end encryption each time you start cloudhsm\_mgmt\_util\.

```
aws-cloudhsm>enable_e2e
E2E enabled on server 0(10.0.2.9)
E2E enabled on server 1(10.0.3.11)
E2E enabled on server 2(10.0.1.12)
```

### Log in to the HSMs<a name="cloudhsm_mgmt_util-log-in"></a>

Use the loginHSM command to log in to the HSMs\. Any user of any type can use this command to log in to the HSMs\. 

The command in following example logs in *admin*, which is the default [crypto officer \(CO\)](hsm-users.md)\. You set this user's password when you [activated the cluster](activate-cluster.md)\. The output shows that the command logged the *admin* user in to all of the HSMs in the cluster\.

**Warning**  
When you log in to cloudhsm\_mgmt\_util, verify that the ENI IP addresses in the success messages *exactly match* the ENI IP addresses of all HSMs in the cluster\. If they do not, stop and run all steps in the [Prepare to run cloudhsm\_mgmt\_util](#cloudhsm_mgmt_util-setup) procedure\.   
To get the ENI IP addresses of the HSMs in your cluster, the [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation, the [describe\-clusters](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cli-guide.htmldescribe-clusters.html) command, or the [Get\-HSM2Cluster](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\.

```
aws-cloudhsm>loginHSM CO admin <password>
loginHSM success on server 0(10.0.2.9)
loginHSM success on server 1(10.0.3.11)
loginHSM success on server 2(10.0.1.12)
```

The following shows the syntax for the loginHSM command\.

```
aws-cloudhsm>loginHSM <user type> <user name> <password>
```

### Log Out from the HSMs<a name="cloudhsm_mgmt_util-log-out"></a>

Use the logoutHSM command to log out of the HSMs\.

```
aws-cloudhsm>logoutHSM
logoutHSM success on server 0(10.0.2.9)
logoutHSM success on server 1(10.0.3.11)
logoutHSM success on server 2(10.0.1.12)
```

### Stop cloudhsm\_mgmt\_util<a name="cloudhsm_mgmt_util-stop"></a>

Use the quit command to stop cloudhsm\_mgmt\_util\.

```
aws-cloudhsm>quit


disconnecting from servers, please wait...
```