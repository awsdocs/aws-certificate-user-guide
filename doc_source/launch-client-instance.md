# Launch a Client<a name="launch-client-instance"></a>

To interact with and manage your AWS CloudHSM cluster and HSM instances, you must be able to communicate with the elastic network interfaces \(ENIs\) of your HSMs\. The easiest way to do this is to use an Amazon EC2 instance in the same Amazon VPC as your cluster \(see below\)\. You can also use the following AWS resources to connect to your cluster\. 

+ [Amazon VPC Peering](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/Welcome.html)

+ [AWS Direct Connect](https://aws.amazon.com/documentation/direct-connect/)

+ [VPN Connections](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html)

## Launch an EC2 Client<a name="launch-client-instance-ec2"></a>

 The AWS CloudHSM documentation typically assumes that you are using an Amazon EC2 instance in the same Amazon Virtual Private Cloud \(VPC\) and Availability Zone \(AZ\) in which you create your cluster\. For more information about creating an Amazon EC2 Linux client if you don't already have one, see [Getting Started with Amazon EC2 Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\. For information about connecting to a running client, see the following topics: 

+ [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

+ [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

Note that you can use your Amazon EC2 instance to run all of the AWS CLI commands contained in this guide\. You can also install the AWS CloudHSM client software on your instance\. For more information, see [Install the CloudHSM Client](install-and-configure-client.md)\. 