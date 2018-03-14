# Keep HSM Users In Sync Across HSMs In The Cluster<a name="troubleshooting-keep-hsm-users-in-sync"></a>

To [manage your HSM's users](manage-hsm-users.md), you use a AWS CloudHSM command line tool known as cloudhsm\_mgmt\_util\. It communicates only with the HSMs that are in the tool's configuration file\. It's not aware of other HSMs in the cluster that are not in the configuration file\.

AWS CloudHSM synchronizes the keys on your HSMs across all other HSMs in the cluster, but it doesn't synchronize the HSM's users or policies\. When you use cloudhsm\_mgmt\_util to [manage HSM users](manage-hsm-users.md), these user changes might affect only some of the cluster's HSMs—the ones that are in the cloudhsm\_mgmt\_util configuration file\. This can cause problems when AWS CloudHSM syncs keys across HSMs in the cluster, because the users that own the keys might not exist on all HSMs in the cluster\.

To avoid these problems, edit the cloudhsm\_mgmt\_util configuration file *before* managing users\. For more information, see [Step 4: Update the cloudhsm\_mgmt\_util Configuration File](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-update-configuration)\.