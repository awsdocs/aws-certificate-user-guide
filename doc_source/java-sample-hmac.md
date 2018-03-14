# Create an HMAC<a name="java-sample-hmac"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This sample shows how to generate a hash\-based message authentication code \(HMAC\) in the HSM and use it to hash a message\. Unlike a typical hash, an HMAC uses a hash function and a cryptographic key\. 

To generate a symmetric key, this sample calls the `generateAESKey()` method in the [Create an AES Key](java-sample-aes-key.md) sample\. It uses the `loginWithExplicitCredentials()` method in the [Log In To an HSM](java-sample-login.md) example to log in to the HSM, but you can substitute the login method that you prefer\. Also, this sample assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;

import javax.crypto.Mac;
import javax.xml.bind.DatatypeConverter;

import com.cavium.key.CaviumAESKey;

public class HMACExample {

    String message  = "This is a plaintext message.";
    String macAlgorithm= "HmacSHA512";

    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        
        // Generate a 256-bit AES key for the HMAC.
        Key aesKey = new SymmetricKeyGeneration().generateAESKey(256, true);
        HMACExample obj = new HMACExample();
        
        // Generate the HMAC using the plaintext, algorithm, and key.
        byte[] mac= obj.getHmac(obj.message, obj.macAlgorithm, (CaviumAESKey)aesKey);
        System.out.println("HMAC : " + DatatypeConverter.printHexBinary(mac));
        LoginLogoutExample.logout();
    }
    
    // The HMAC function takes a hash algorithm and a cryptographic key.
    public byte[] getHmac(String message, String macAlgorithm, CaviumAESKey key) {
        try {
            Mac mac = Mac.getInstance( macAlgorithm,"Cavium");
            mac.init(key);
            mac.update(message.getBytes());
            byte[] hmacValue = mac.doFinal();
            return hmacValue;

        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        }
        return null;

    }
}
```