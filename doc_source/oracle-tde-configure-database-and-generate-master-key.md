# Oracle TDE with AWS CloudHSM: Configure the Database and Generate the Master Encryption Key<a name="oracle-tde-configure-database-and-generate-master-key"></a>

To integrate Oracle TDE with your AWS CloudHSM cluster, see the following topics:

1. [Update the Oracle Database Configuration](#oracle-tde-configure-database) to use the HSMs in your cluster as the *external security module*\. For information about external security modules, see [Introduction to Transparent Data Encryption](https://docs.oracle.com/database/122/ASOAG/introduction-to-transparent-data-encryption.htm) in the *Oracle Database Advanced Security Guide*\.

1. [Generate the Oracle TDE Master Encryption Key](#oracle-tde-generate-master-key) on the HSMs in your cluster\.


+ [Update the Oracle Database Configuration](#oracle-tde-configure-database)
+ [Generate the Oracle TDE Master Encryption Key](#oracle-tde-generate-master-key)

## Update the Oracle Database Configuration<a name="oracle-tde-configure-database"></a>

To update the Oracle Database configuration to use an HSM in your cluster as the *external security module*, complete the following steps\. For information about external security modules, see [ Introduction to Transparent Data Encryption ](https://docs.oracle.com/database/122/ASOAG/introduction-to-transparent-data-encryption.htm) in the *Oracle Database Advanced Security Guide*\. 

**To update the Oracle configuration**

1. Connect to your Amazon EC2 client instance\. This is the instance where you installed Oracle Database\.

1. Make a backup copy of the file named `sqlnet.ora`\. For the location of this file, see the Oracle documentation\. 

1. Use a text editor to edit the file named `sqlnet.ora`\. Add the following line\. If an existing line in the file begins with `encryption_wallet_location`, replace the existing line with the following one\.

   ```
   encryption_wallet_location=(source=(method=hsm))
   ```

   Save the file\.

1. Run the following command to create the directory where Oracle Database expects to find the library file for the AWS CloudHSM PKCS \#11 software library\. 

   ```
   sudo mkdir -p /opt/oracle/extapi/64/hsm
   ```

1. Use one of the following commands to copy the AWS CloudHSM software library for PKCS \#11 file to the directory that you created in the previous step\. 

   + If you installed the PKCS \#11 library without Redis, run the following command\.

     ```
     sudo cp /opt/cloudhsm/lib/libcloudhsm_pkcs11_standard.so /opt/oracle/extapi/64/hsm/
     ```

   + If you installed the PKCS \#11 library with Redis, run the following command\.

     ```
     sudo cp /opt/cloudhsm/lib/libcloudhsm_pkcs11_redis.so /opt/oracle/extapi/64/hsm/
     ```
**Note**  
The `/opt/oracle/extapi/64/hsm` directory must contain only one library file\. Copy only the library file that corresponds to the way you [installed the PKCS \#11 library](pkcs11-library-install.md#install-pkcs11-library)\. If additional files exist in that directory, remove them\. 

1. Run the following command to change the ownership of the `/opt/oracle` directory and everything inside it\.

   ```
   sudo chown -R oracle:dba /opt/oracle
   ```

1. Start the Oracle Database\.

## Generate the Oracle TDE Master Encryption Key<a name="oracle-tde-generate-master-key"></a>

To generate the Oracle TDE master key on the HSMs in your cluster, complete the steps in the following procedure\.

**To generate the master key**

1. Use the sqlplus command to open Oracle SQL\*Plus\. When prompted, type the system password that you set when you installed Oracle Database\. 

1. Run the SQL statement that creates the master encryption key, as shown in the following examples\. Use the statement that corresponds to your version of Oracle Database\. Replace *<CU user name>* with the user name of the cryptographic user \(CU\)\. Replace *<password>* with the CU password\. 
**Important**  
Run the following command only once\. Each time the command is run, it creates a new master encryption key\. 

   + For Oracle Database version 11, run the following SQL statement\.

     ```
     SQL> alter system set encryption key identified by "<CU user name>:<password>";
     ```

   + For Oracle Database version 12, run the following SQL statement\.

     ```
     SQL> administer key management set key identified by "<CU user name>:<password>";
     ```

   If the response is `System altered` or `keystore altered`, then you successfully generated and set the master key for Oracle TDE\. 

1. \(Optional\) Run the following command to verify the status of the *Oracle wallet*\.

   ```
   SQL> select * from v$encryption_wallet;
   ```

   If the wallet is not open, use one of the following commands to open it\. Replace *<CU user name>* with the name of the cryptographic user \(CU\)\. Replace *<password>* with the CU password\. 

   + For Oracle 11, run the following command to open the wallet\.

     ```
     SQL> alter system set encryption wallet open identified by "<CU user name>:<password>";
     ```

     To manually close the wallet, run the following command\.

     ```
     SQL> alter system set encryption wallet close identified by "<CU user name>:<password>";
     ```

   + For Oracle 12, run the following command to open the wallet\.

     ```
     SQL> administer key management set keystore open identified by "<CU user name>:<password>";
     ```

     To manually close the wallet, run the following command\.

     ```
     SQL> administer key management set keystore close identified by "<CU user name>:<password>";
     ```