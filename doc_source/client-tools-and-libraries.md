# AWS CloudHSM Client Tools and Software Libraries<a name="client-tools-and-libraries"></a>

To manage and use the HSMs in your cluster, you use the AWS CloudHSM client software\. The client software includes several components, as described in the following topics\.


+ [AWS CloudHSM Client](#client)
+ [AWS CloudHSM Command Line Tools](#command-line-tools-intro)
+ [AWS CloudHSM Software Libraries](#software-libraries)

## AWS CloudHSM Client<a name="client"></a>

The AWS CloudHSM client is a daemon that you install and run on your application hosts\. The client establishes and maintains a secure, end\-to\-end encrypted connection with the HSMs in your AWS CloudHSM cluster\. The client provides the fundamental connection between your application hosts and your HSMs\. Most of the other AWS CloudHSM client software components rely on the client to communicate with your HSMs\. To get started with the AWS CloudHSM client, see [Install the CloudHSM Client](install-and-configure-client.md)\.

### AWS CloudHSM Client End\-to\-End Encryption<a name="client-end-to-end-encryption"></a>

Communication between the AWS CloudHSM client and the HSMs in your cluster is encrypted from end to end\. Only your client and your HSMs can decrypt the communication\.

The following process explains how the client establishes end\-to\-end encrypted communication with an HSM\.

1. Your client establishes a Transport Layer Security \(TLS\) connection with the server that hosts your HSM hardware\. Your cluster's security group allows inbound traffic to the server only from client instances in the security group\. The client also checks the server's certificate to ensure that it's a trusted server\.  
![\[A TLS connection between the client and the server.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/end-to-end-encryption-part-1.png)

1. Next, the client establishes an encrypted connection with the HSM hardware\. The HSM has the cluster certificate that you signed with your own certificate authority \(CA\), and the client has the CA's root certificate\. Before the client–HSM encrypted connection is established, the client verifies the HSM's cluster certificate against its root certificate\. The connection is established only when the client successfully verifies that the HSM is trusted\. The client–HSM encrypted connection goes through the client–server connection established previously\.  
![\[A secure, end-to-end encrypted connection between the client and the HSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/end-to-end-encryption-part-2.png)

## AWS CloudHSM Command Line Tools<a name="command-line-tools-intro"></a>

The AWS CloudHSM client software includes two command line tools\. You use the command line tools to manage the users and keys on the HSMs\. For example, you can create HSM users, change user passwords, create keys, and more\. For information about these tools, see [Command Line Tools](command-line-tools.md)\.

## AWS CloudHSM Software Libraries<a name="software-libraries"></a>

You can use the AWS CloudHSM software libraries to integrate your applications with the HSMs in your cluster and use them for cryptoprocessing\. For more information about installing and using the different libraries, see [Using the Software Libraries](use-hsm.md)\.