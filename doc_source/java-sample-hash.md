# Create a Hash<a name="java-sample-hash"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This sample shows how to generate a hash of a message using an HSM and the SHA\-512 hash algorithm\.

This example uses the `loginWithExplicitCredentials()` method in the [Log In To and Out Of an HSM](java-sample-login.md) sample to log in to the HSM, but you can substitute the login method that you prefer\. Also, the sample assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

```
package com.amazonaws.cloudhsm.examples;

import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import javax.xml.bind.DatatypeConverter;

public class HashExample {
    String plainText = "This is a sample plaintext message.";
    String hashAlgorithm = "SHA-512";
    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        HashExample obj = new HashExample();
        // Generate the hash.
        byte[] hash = obj.getHash(obj.plainText, obj.hashAlgorithm);

        System.out.println("Hash : " + DatatypeConverter.printHexBinary(hash));
        LoginLogoutExample.logout();
    }

    public byte[] getHash(String message, String hashAlgorithm) {
        try {
            // Specify the Cavium provider.
            MessageDigest md = MessageDigest.getInstance(hashAlgorithm, "Cavium");
            md.update(message.getBytes());
            byte[] hash = md.digest();
            return hash;
        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```