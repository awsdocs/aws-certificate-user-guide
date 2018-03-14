# Install and Use the AWS CloudHSM Software Library for Java<a name="java-library-install"></a>

Before you can use the AWS CloudHSM software library for Java, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster, and the Java library communicates locally with the client\. If you haven't installed and configured the AWS CloudHSM client package, do that now by following the steps at [Install the CloudHSM Client](install-and-configure-client.md)\. After you install and configure the client, use the following command to start it\.

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


+ [Installing the Java Library](#install-java-library)
+ [Testing the Java Library](#test-java-library)
+ [Providing Credentials to the Java Library](#java-library-credentials)
+ [Key Management Basics in the Java Library](#java-library-key-basics)

## Installing the Java Library<a name="install-java-library"></a>

Complete the following steps to install or update the AWS CloudHSM software library for Java\.

Use the following commands to download and install the Java library\.

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce-latest.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-jce-latest.x86_64.rpm
```

------
#### [ Ubuntu ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-jce_latest_amd64.deb
```

```
$ sudo dpkg -i cloudhsm-client-jce_latest_amd64.deb
```

------

After you complete the preceding steps, you can find the following Java library files:

+ `/opt/cloudhsm/java/cloudhsm-1.0.jar`

+ `/opt/cloudhsm/java/cloudhsm-test-1.0.jar`

+ `/opt/cloudhsm/java/hamcrest-all-1.3.jar`

+ `/opt/cloudhsm/java/junit.jar`

+ `/opt/cloudhsm/java/log4j-api-2.8.jar`

+ `/opt/cloudhsm/java/log4j-core-2.8.jar`

+ `/opt/cloudhsm/lib/libcaviumjca.so`

## Testing the Java Library<a name="test-java-library"></a>

To test that the AWS CloudHSM software library for Java works with the HSMs in your cluster, complete the following steps\.

**To test the Java library with your cluster**

1. \(Optional\) If you don't already have Java installed in your environment, use the following command to install it\.

------
#### [ Amazon Linux ]

   ```
   $ sudo yum install -y java-1.8.0-openjdk
   ```

------
#### [ Ubuntu ]

   ```
   $ sudo apt-get install openjdk-8-jre
   ```

------

1. Use the following commands to set the necessary environment variables\. Replace *<HSM user name>* and *<password>* with the credentials of a crypto user \(CU\)\.

   ```
   $ export LD_LIBRARY_PATH=/opt/cloudhsm/lib
   ```

   ```
   $ export HSM_PARTITION=PARTITION_1
   ```

   ```
   $ export HSM_USER=<HSM user name>
   ```

   ```
   $ export HSM_PASSWORD=<password>
   ```

1. Use the following command to run the RSA test\.

   ```
   $ java8 -cp "/usr/share/java/junit4.jar:/opt/cloudhsm/java/*" \
               org.junit.runner.JUnitCore \
               com.cavium.unittest.TestRSA
   ```

   To run a different test, replace `TestRSA` in the preceding command with one of the following values:

   + `TestAESKeyGen`

   + `TestAes`

   + `TestBlockCipherBuffer`

   + `TestKeyStore`

   + `TestLoginManager`

   + `TestMac`

   + `TestMessageDigest`

   + `TestMessageUtil`

   + `TestPadding`

   + `TestProvider`

   + `TestRSA`

   + `TestRSAKeyGen`

   + `TestSUNJce`

   + `TestUtils`

## Providing Credentials to the Java Library<a name="java-library-credentials"></a>

Your Java application must be authenticated by the HSMs in your cluster before it can use them\. Each application can use one session, which is established by providing credentials in one of the following ways\. In the following examples, replace *<HSM user name>* and *<password>* with the credentials of a crypto user \(CU\)\.

The first of the following examples shows how to use the `LoginManager` class to manage sessions in your code\. Instead, you can let the library implicitly manage sessions when your application starts, as shown in the remaining examples\. However in these latter cases it might be difficult to understand error conditions when the provided credentials are invalid or the HSMs are having problems\. When you use the `LoginManager` class, you have more control over how your application deals with failures\.

+ Use the `LoginManager` class to provide credentials directly in your application\. For example:

  ```
  LoginManager lm = LoginManager.getInstance();
  lm.loadNative();
  lm.login("PARTITION_1", "<HSM user name>", "<password>");
  ```

+ Provide a file named `HsmCredentials.properties` in your application's `CLASSPATH`\. The file's contents should look like the following:

  ```
  HSM_PARTITION = PARTITION_1
  HSM_USER = <HSM user name>
  HSM_PASSWORD = <password>
  ```

+ Provide Java system properties when running your application\. The following examples show two different ways that you can do this:

  ```
  $ java -DHSM_PARTITION=PARTITION_1 -DHSM_USER=<HSM user name> -DHSM_PASSWORD=<password>
  ```

  ```
  System.setProperty("HSM_PARTITION","PARTITION_1");
  System.setProperty("HSM_USER","<HSM user name>");
  System.setProperty("HSM_PASSWORD","<password>");
  ```

+ Set system environment variables\. For example:

  ```
  $ export HSM_PARTITION=PARTITION_1
  ```

  ```
  $ export HSM_USER=<HSM user name>
  ```

  ```
  $ export HSM_PASSWORD=<password>
  ```

## Key Management Basics in the Java Library<a name="java-library-key-basics"></a>

The following key management basics can help you get started with the AWS CloudHSM software library for Java\.

**To import a key implicitly**  
To implicitly import a key, pass the key to any API that accepts one\. If the key is the correct type for the specified operation, the HSMs automatically import and use the provided key\.

**To import a key explicitly**  
Use the utility class named `ImportKey` to import a key and set its attributes\.

**To make a session key persist**  
Use the `Util.persistKey()` method to make a session key into a token key—that is, to persist the key in the HSMs\.

**To delete a key**  
Use the `Util.deleteKey()` method to delete a key\.