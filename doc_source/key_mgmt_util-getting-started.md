# Getting Started with key\_mgmt\_util<a name="key_mgmt_util-getting-started"></a>

AWS CloudHSM includes two command line tools with the [AWS CloudHSM client software](install-and-configure-client.md#install-client)\. The [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-reference.md) tool includes commands to manage HSM users\. The [key\_mgmt\_util](key_mgmt_util-reference.md)tool includes commands to manage keys\. To get started with the key\_mgmt\_util command line tool, see the following topics\. 


+ [Set Up key\_mgmt\_util](#key_mgmt_util-setup)
+ [Basic Usage of key\_mgmt\_util](#key_mgmt_util-basics)

If you encounter an error message or unexpected outcome for any command, see the [Troubleshooting AWS CloudHSM](troubleshooting.md) topics for help\. For details about the key\_mgmt\_util commands, see [key\_mgmt\_util Command Reference](key_mgmt_util-reference.md) 

## Set Up key\_mgmt\_util<a name="key_mgmt_util-setup"></a>

Complete the following setup before you use key\_mgmt\_util\.

### Start the AWS CloudHSM Client<a name="key_mgmt_util-start-cloudhsm-client"></a>

Before you use key\_mgmt\_util, you must start the AWS CloudHSM client\. The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster\. The key\_mgmt\_util tool uses the client connection to communicate with the HSMs in your cluster\. Without it, key\_mgmt\_util doesn't work\. 

**To start the AWS CloudHSM client**  
Use the following command to start the AWS CloudHSM client\.

------
#### [ Amazon Linux ]

```
$ sudo start cloudhsm-client
```

------
#### [ Ubuntu ]

```
$ sudo service cloudhsm-client start
```

------

### Start key\_mgmt\_util<a name="key_mgmt_util-start"></a>

After you start the AWS CloudHSM client, use the following command to start key\_mgmt\_util\.

```
$ /opt/cloudhsm/bin/key_mgmt_util
```

The prompt changes to Command: when key\_mgmt\_util is running\.

If the command fails, such as returning a `Daemon socket connection error` message, try [updating your configuration file](troubleshooting-lost-connection.md)\. 

## Basic Usage of key\_mgmt\_util<a name="key_mgmt_util-basics"></a>

See the following topics for the basic usage of the key\_mgmt\_util tool\.


+ [Log In to the HSMs](#key_mgmt_util-log-in)
+ [Log Out from the HSMs](#key_mgmt_util-log-out)
+ [Stop key\_mgmt\_util](#key_mgmt_util-stop)

### Log In to the HSMs<a name="key_mgmt_util-log-in"></a>

Use the loginHSM command to log in to the HSMs\. The following command logs in as a [crypto user \(CU\)](hsm-users.md) named `example_user`\. The output indicates a successful login for all three HSMs in the cluster\. 

```
Command:  loginHSM -u CU -s example_user -p <password>
Cfm3LoginHSM returned: 0x00 : HSM Return: SUCCESS

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

The following shows the syntax for the loginHSM command\.

```
Command:  loginHSM -u <user type> -s <username> -p <password>
```

### Log Out from the HSMs<a name="key_mgmt_util-log-out"></a>

Use the logoutHSM command to log out from the HSMs\.

```
Command:  logoutHSM
Cfm3LogoutHSM returned: 0x00 : HSM Return: SUCCESS

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

### Stop key\_mgmt\_util<a name="key_mgmt_util-stop"></a>

Use the exit command to stop key\_mgmt\_util\.

```
Command:  exit
```