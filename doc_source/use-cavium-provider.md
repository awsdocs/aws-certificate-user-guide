# Specify the Cavium Provider<a name="use-cavium-provider"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

The Java examples included in this documentation use the Cavium provider in the AWS CloudHSM client package\. To specify the Cavium provider, use either of the following techniques: 

+ Create an instance of the Cavium provider and pass it to the methods that take a provider, such as this `KeyGenerator.getInstance()` method\.

  ```
  CaviumProvider cp = new CaviumProvider();
  keyGen = KeyGenerator.getInstance("AES",cp);
  ```

+ Add the Cavium provider to the `$JAVA_HOME/jre/lib/security/java.security` file\. Then use the **Cavium** string to refer to the provider\. If you do not specify a provider, Java uses the first provider in the file, but it's best to specify the provider explicitly\. 
**Important**  
The Cavium JCA provider cannot be installed as the highest priority provider\.

  ```
  // Add the Cavium provider to the Java provider file but make 
  // sure that it's not in the first slot within the file.
  Security.addProvider(new CaviumProvider());
  
  // Or insert the provider into any specific position other than the first.
  Security.insertProviderAt(new CaviumProvider(), 10);
  
  // Then you can use "Cavium" where necessary to specify the provider.
  keyGen = KeyGenerator.getInstance("AES","Cavium");
  ```