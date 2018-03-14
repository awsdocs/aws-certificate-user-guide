# Signing a Message<a name="java-sample-sign-message"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This sample shows how to sign a message by using a key in an HSM\. The sample generates a 4096\-bit asymmetric key pair\. It uses the private key to sign the message\. It uses the public key to verify the message signature\. 

To generate the key pair, this sample calls the `generateRSAKeyPair()` method in the [Create an RSA Key Pair](java-sample-rsa-key.md) sample\. It uses the `loginWithExplicitCredentials()` method in the [Log In To an HSM](java-sample-login.md) sample to log in to the HSM, but you can substitute the login method that you prefer\. Also, the sample assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyPair;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.Signature;
import java.security.SignatureException;
import java.util.Base64;

import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;

public class SignatureExample {
    
    String sampleMessage = "This is a sample message.";
    String signingAlgorithmn = "SHA512withRSA/PSS";
    public static void main(String[] args) {
        LoginLogoutExample.loginUsingJavaProperties();
        SignatureExample obj = new SignatureExample();
        
        // Generate a 4096-bit pair and save it in the HSM. 
        KeyPair kp = new AsymmetricKeyGeneration().generateRSAKeyPair(4096, true);
        System.out.println("Generated key pair");
        
        // Sign the message with the private key and the specified signing algorithm.
        byte[] signature = obj.signMessage(obj.sampleMessage, obj.signingAlgorithm, (CaviumRSAPrivateKey)kp.getPrivate());
        System.out.println("Signature : " + Base64.getEncoder().encodeToString(signature));
        
        // Verify the signature.
        boolean isVerificationSuccessful = obj.verifySign(obj.sampleMessage, obj.signingAlgo, (CaviumRSAPublicKey)kp.getPublic(), signature);
        System.out.println("Verification result : " + isVerificationSuccessful);
        LoginLogoutExample.logout();
    }
    
    // Use the private key to sign the message.
    public byte[] signMessage(String message, String signingAlgorithm, CaviumRSAPrivateKey privateKey) {
        try {
            Signature sig = Signature.getInstance(signingAlgorithm, "Cavium");
            sig.initSign(privateKey);
            sig.update(message.getBytes()); 
            byte[] signature = null;
            signature = sig.sign();
            return signature;

        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (SignatureException e) {
            e.printStackTrace();
        }
        return null;
    }

    // Use the public key to verify the message signature.
    public boolean verifySign(String message, String signingAlgorithm, CaviumRSAPublicKey publicKey, byte[] signature) {
        try {
            Signature sig = Signature.getInstance(signingAlgorithm, "Cavium");
            sig.initVerify(publicKey);
            sig.update(message.getBytes());
            
            boolean isVerificationSuccessful = sig.verify(signature);
            return isVerificationSuccessful;
        } catch (NoSuchAlgorithmException | NoSuchProviderException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (SignatureException e) {
            e.printStackTrace();
        }
        return false;
    }
}
```