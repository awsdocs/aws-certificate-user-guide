# Oracle Database Transparent Data Encryption \(TDE\) with AWS CloudHSM<a name="oracle-tde"></a>

Some versions of Oracle's database software offer a feature called Transparent Data Encryption \(TDE\)\. With TDE, the database software encrypts data before storing it on disk\. The data in the database's table columns or tablespaces is encrypted with a table key or tablespace key\. These keys are encrypted with the TDE master encryption key\. You can store the TDE master encryption key in the HSMs in your AWS CloudHSM cluster, which provides additional security\.

![\[Store the Oracle TDE master encryption key in AWS CloudHSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/tde-master-key-in-hsm.png)

In this solution, you use Oracle Database installed on an Amazon EC2 instance\. Oracle Database integrates with the [AWS CloudHSM software library for PKCS \#11](pkcs11-library.md) to store the TDE master key in the HSMs in your cluster\.

**Important**  
You cannot use an Oracle instance in Amazon Relational Database Service \(Amazon RDS\) to integrate with AWS CloudHSM\. You must install Oracle Database on an Amazon EC2 instance\.  
For information about integrating an Oracle instance in Amazon RDS with AWS CloudHSM Classic, see [Using AWS CloudHSM Classic to Store Amazon RDS Oracle TDE Keys](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.OracleCloudHSM.html) in the *Amazon Relational Database Service User Guide*\.

Complete the following steps to accomplish Oracle TDE integration with AWS CloudHSM\.

**To configure Oracle TDE integration with AWS CloudHSM**

1. Follow the steps in [Set Up Prerequisites](oracle-tde-prerequisites.md) to prepare your environment\.

1. Follow the steps in [Configure the Database](oracle-tde-configure-database-and-generate-master-key.md) to configure Oracle Database to integrate with your AWS CloudHSM cluster\.