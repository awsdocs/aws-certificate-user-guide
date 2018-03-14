# Install and Configure the AWS CloudHSM Client<a name="install-and-configure-client"></a>

To interact with the HSM in your AWS CloudHSM cluster, you need the AWS CloudHSM client software\. We recommend that you install it on [the client instance that you created previously](launch-client-instance.md)\. 


+ [Install the AWS CloudHSM Client and Command Line Tools](#install-client)
+ [Edit the Client Configuration](#edit-client-configuration)

## Install the AWS CloudHSM Client and Command Line Tools<a name="install-client"></a>

Complete the steps in the following procedure to install the AWS CloudHSM client and command line tools\.

**To install \(or update\) the client and command line tools**

1. Connect to your client instance\.

1. Use the following commands to download and then install the client and command line tools\.

------
#### [ Amazon Linux ]

   ```
   wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-latest.x86_64.rpm
   ```

   ```
   sudo yum install -y ./cloudhsm-client-latest.x86_64.rpm
   ```

------
#### [ Ubuntu ]

   ```
   wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client_latest_amd64.deb
   ```

   ```
   sudo dpkg -i cloudhsm-client_latest_amd64.deb
   ```

------

## Edit the Client Configuration<a name="edit-client-configuration"></a>

Before you can use the AWS CloudHSM client to connect to your cluster, you must edit the client configuration\. 

**To edit the client configuration**

1. Copy your issuing certificate—[the one that you used to sign the cluster's certificate](initialize-cluster.md#sign-csr)—to the following location on the client instance: `/opt/cloudhsm/etc/customerCA.crt`\. You need root permissions on the client instance to copy your certificate to this location\. 

1. Use the following [configure](configure-tool.md) command to update the configuration files for the AWS CloudHSM client and command line tools, specifying the IP address of the HSM in your cluster\. To get the HSM's IP address, view your cluster in the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), or run the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) AWS CLI command\. In the command's output, the HSM's IP address is the value of the `EniIp` field\. If you have more than one HSM, choose the IP address for any of the HSMs; it doesn't matter which one\. 

   ```
   sudo /opt/cloudhsm/bin/configure -a <IP address>
   
   Inserting '<IP address>' into '/opt/cloudhsm/etc/cloudhsm_client.cfg'
   Inserting '<IP address>' into '/opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg'
   ```

After you install and configure the AWS CloudHSM client, proceed to [Activate the Cluster](activate-cluster.md)\. 