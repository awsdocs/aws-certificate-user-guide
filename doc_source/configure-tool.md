# Configure Tool<a name="configure-tool"></a>

AWS CloudHSM automatically synchronizes data among all HSMs in a cluster\. The Configure tool updates the HSM data in the configuration files that the synchronization mechanisms use\. Use Configure to refresh the HSM data before you use the command line tools, especially when the HSMs in the cluster have changed\. 

Before running key\_mgmt\_util or cloudhsm\_mgmt\_util, use the following Configure commands\.

+ [key\_mgmt\_util](key_mgmt_util.md): Before using key\_mgmt\_util, run `configure -a`\.

+ [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md): Before using cloudhsm\_mgmt\_util, run `configure -a`, then `configure -m`\.

## Syntax<a name="configure-tool-syntax"></a>

```
configure -h | --help

configure -a <ENI IP address>

configure -m [-i <daemon_id>]
```

## Examples<a name="configure-tool-examples"></a>

These examples show how to use the Configure tool\.

**Example : Update the HSM Data for the AWS CloudHSM Client and key\_mgmt\_util**  
This example uses the `configure -a` command to update the HSM data for the AWS CloudHSM client and key\_mgmt\_util\. This command is also the first step in updating the cloudhsm\_mgmt\_util configuration file\.  
Before running `configure -a`, stop the AWS CloudHSM client\. This prevents conflicts that might occur while Configure edits the client's configuration file\. If the client is already stopped, this command has no ill effects, so you can use it in a script\.  

```
$  sudo stop cloudhsm-client
cloudhsm-client stop/waiting
```
Next, get the ENI IP address of any one of the HSMs in your cluster\. This command uses the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command in the AWS CLI, but you can also use the [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation or the [Get\-HSM2Cluster](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\.  
This excerpt of the output shows the ENI IP addresses of the HSMs in a sample cluster\. We can use either of the IP addresses in the next command\.  

```
$  aws cloudhsmv2 describe-clusters

{
    "Clusters": [
        { ... }
            "Hsms": [
                {
...
                    "EniIp": "10.0.0.9",
...
                },
                {
...
                    "EniIp": "10.0.1.6",
...
```
This step uses configure \-a to add the `10.0.0.9` ENI IP address to the configurations files\.   
The output shows that Configure added `10.0.0.9` to the `cloudhsm_client.cfg` and `cloudhsm_mgmt_util.cfg` files\.  

```
$  sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
Updating server config in /opt/cloudhsm/etc/cloudhsm_client.cfg
Updating server config in /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
```
Next, restart the AWS CloudHSM client\. When the client starts, it uses the ENI IP address in its configuration file to query the cluster\. Then, it writes the ENI IP addresses of all HSMs in the cluster to the `cluster.info` file\.  

```
$  sudo start cloudhsm-client
cloudhsm-client start/running, process 2747
```
When the command completes, the HSM data that the AWS CloudHSM client and key\_mgmt\_util use is complete and accurate\. Before using cloudhsm\_mgmt\_util, run the configure \-m command, as shown in the following example\.

**Example : Update the HSM Data for cloudhsm\_mgmt\_util**  
This example uses the configure \-m command to copy the update the updated HSM data from the `cluster.info` file to the `cloudhsm_mgmt_util.cfg` file that cloudhsm\_mgmt\_util uses\.  
Before running configure \-m, stop the AWS CloudHSM client, run `configure -a`, and then restart the AWS CloudHSM client, as shown in the [previous example](#configure-tool-examples)\. This ensures that the data copied into the `cloudhsm_mgmt_util.cfg` file from the `cluster.info` file is complete and accurate\.  

```
$  sudo /opt/cloudhsm/bin/configure -m
Updating '/opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg' from cluster state
```

## Parameters<a name="configure-tool-params"></a>

**\-h | \-\-help**  
Displays command syntax\.  
Required: Yes

**\-a *<ENI IP address>***  
Adds the specified HSM elastic network interface \(ENI\) IP address to AWS CloudHSM configuration files\. Enter the ENI IP address of any one of the HSMs in the cluster\. It does not matter which one you select\.   
To get the ENI IP addresses of the HSMs in your cluster, use the [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation, the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) AWS CLI command, or the [Get\-HSM2Cluster](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\.  
Before running configure \-a, stop the AWS CloudHSM client\. Then, when configure \-a completes, restart the AWS CloudHSM client\. For details, [see the examples](#configure-tool-examples)\.
This parameter edits the following configuration files:  

+ `/opt/cloudhsm/etc/cloudhsm_client.cfg`: Used by AWS CloudHSM client and [key\_mgmt\_util](key_mgmt_util.md)\.

+ `/opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg`: Used by [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\.
When the AWS CloudHSM client starts, it uses the ENI IP address in its configuration file to query the cluster and update the `cluster.info` file \(`/opt/cloudhsm/daemon/1/cluster.info`\) with the correct ENI IP addresses for all HSMs in the cluster\.  
Required: Yes

**\-m**  
Updates the HSM ENI IP addresses in the configuration file that cloudhsm\_mgmt\_util uses\.   
When you run configure \-a and then start the AWS CloudHSM client, the client daemon queries the cluster and updates the `cluster.info` files with the correct HSM IP addresses for all HSMs in the cluster\. Running configure \-m completes the update by copying the HSM IP addresses from the `cluster.info` to the `cloudhsm_mgmt_util.cfg` configuration file that cloudhsm\_mgmt\_util uses\.  
Be sure to run configure \-a and restart the AWS CloudHSM client before running configure \-m\. This ensures that the data copied into `cloudhsm_mgmt_util.cfg` from `cluster.info` is complete and accurate\.  
Required: Yes

**\-i**  
Specifies an alternate client daemon\. The default value represents the AWS CloudHSM client\.  
Default: `1`  
Required: No

## Related Topics<a name="configure-tool-seealso"></a>

+ [Set Up key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-setup)

+ [Prepare to run cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup)