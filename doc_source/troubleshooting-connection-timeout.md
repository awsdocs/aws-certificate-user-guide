# EC2 SSH Connection Times Out<a name="troubleshooting-connection-timeout"></a>

The [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) command line tool includes commands that enable you to manage HSM users\. When you [start the `cloudhsm_mgmt_util` utility](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start), it attempts to connect to your Amazon EC2 instance\. If your connection times out, check to determine whether your security group is correctly configured\. 

```
/opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg


Connecting to the server(s), it may take time
depending on the server(s) load, please wait...

Connecting to server 'server1': hostname 'xx.xx.xx.xx', port 2225...
Connection to server xx.xx.xx.xx(xx.xx.xx.xx) on port 2225 failed:Connection timed out
```

If the `cloudhsm_mgmt_util` command line tool can't connect to your Amazon EC2 instance, make sure that the security group the instance is configured to make port 22 available for SSH\. 

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Security Groups**\.

1. Select the security group that you created for your cluster\. This is not the security group that AWS CloudHSM created\.

1. Choose the **Inbound** tab\.

1. Make sure that you have an inbound rule with the following characteristics:

   + **Type**: SSH

   + **Protocol**: TCP

   + **Port Range**: 22