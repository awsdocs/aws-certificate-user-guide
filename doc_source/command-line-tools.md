# AWS CloudHSM Command Line Tools<a name="command-line-tools"></a>

AWS CloudHSM provides command line tools for managing and using AWS CloudHSM\.


+ [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)
+ [key\_mgmt\_util](key_mgmt_util.md)
+ [Configure Tool](configure-tool.md)

**Manage Clusters and HSMs**  
These tools get, create, delete, and tag AWS CloudHSM clusters and HSMs:  

+ [CloudHSMv2 commands in AWS Command Line Interface \(AWS CLI\)](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/index.html)\. To use these commands, you need to [install](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [configure](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration) AWS CLI\.

+ HSM2 PowerShell cmdlets in the [AWSPowerShell module](https://aws.amazon.com/powershell/)\. These cmdlets are available in a Windows PowerShell module and a cross\-platform PowerShell Core module\.

   

**Manage Users**  
This tool creates and deletes HSM users, including implementing quorum authentication of user management tasks:  

+ [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   

**Manage Keys**  
This tool creates, deletes, imports, and exports symmetric keys and asymmetric key pairs:  

+ [key\_mgmt\_util](key_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   

**Helper Tools**  
These tools help you to use the tools and software libraries\.  

+ [configure](configure-tool.md) updates your CloudHSM client configuration files\. This enables the AWS CloudHSM to synchronize the HSMs in a cluster\.

+ [pkpspeed](troubleshooting-verify-hsm-performance.md) measures the performance of your HSM hardware independent of software libraries\. 

   