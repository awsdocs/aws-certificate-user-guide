# Encrypting and Decrypting with an RSA Key Pair<a name="java-sample-asymmetric-encrypt-decrypt"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This sample shows how to encrypt and decrypt a string using a 2048\-bit RSA key pair\. The sample first encrypts a message by using the public key and then decrypts it by using the private key\. It then encrypts using the private key and decrypts using the public key\. 

To generate the key pair, this sample calls the `generateRSAKeyPair()` method in the [Create an RSA Key Pair](java-sample-rsa-key.md) sample\. It uses the `loginWithExplicitCredentials()` method in the [Log In To and Out Of an HSM](java-sample-login.md) sample to log in to the HSM, but you can substitute the login method that you prefer\. Also, this sample assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyPair;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;

import org.bouncycastle.util.encoders.Base64;

import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;

public class AsymmetricEncryptDecryptExample {
    
    String plainText = "This is a plaintext string";
    
    // Specify the encryption algorithm.
    String transformation  = "RSA/ECB/OAEPWithSHA-224ANDMGF1Padding";
    
    public static void main(String[] args) {
        // Log into the HSM.
        LoginLogoutExample.loginWithExplicitCredentials();
        
        // Generate a 2048-bit RSA key pair and save it in the HSM.
        KeyPair kp = new AsymmetricKeyGeneration().generateRSAKeyPair(2048, true);
        
        // Create an example object.
        AsymmetricEncryptDecryptExample obj = new AsymmetricEncryptDecryptExample();
        
        // Get the private key.
        CaviumRSAPrivateKey privKey = (CaviumRSAPrivateKey) (RSAPrivateKey) kp.getPrivate();
        
        //Get the public key.
        CaviumRSAPublicKey pubKey = (CaviumRSAPublicKey) (RSAPublicKey) kp.getPublic();
        System.out.println("Use the private key to encrypt; use the public key to decrypt");
        
        // Encrypt the plaintext with the private key.
        byte[] cipherText = obj.asymmetricKeyEncryption(obj.transformation, privKey, obj.plainText);
        System.out.println("CipherText = " + Base64.toBase64String(cipherText));
        
        // Decrypt the ciphertext with the public key.
        String plainText = obj.asymmetricKeyDecryption(obj.transformation, pubKey, cipherText);
        System.out.println("PlainText = " + plainText);
        System.out.println("Encrypt with public key; decrypt with private key");
        
        // Encrypt with the public key.
        cipherText = obj.asymmetricKeyEncryption(obj.transformation, pubKey, obj.plainText);
        System.out.println("CipherText = " + Base64.toBase64String(cipherText));
        
        // Decrypt with private key.
        plainText = obj.asymmetricKeyDecryption(obj.transformation, privKey, cipherText);
        System.out.println("PlainText = " + plainText);
        LoginLogoutExample.logout();
    }
    
    // Encrypt with the specified algorithm and key.
    public byte[] asymmetricKeyEncryption(String transformation, Key key, String plainText) { 
        try {
            Cipher cipher = Cipher.getInstance(transformation, "Cavium");
            cipher.init(Cipher.ENCRYPT_MODE, key);
            cipher.update(plainText.getBytes());
            
            // Encrypt the plaintext.
            byte[] cihperText = cipher.doFinal(plainText.getBytes());
            return cihperText;
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (NoSuchProviderException e) {
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            e.printStackTrace();
        } catch (BadPaddingException e) {
            e.printStackTrace();
        }
        return null;
    }
    
    // Decrypt with the specified algorithm and key
    public String asymmetricKeyDecryption(String transformation, Key key, byte[] cipherText) { 
        try {
            Cipher cipher = Cipher.getInstance(transformation, "Cavium");
            cipher.init(Cipher.DECRYPT_MODE, key);
            
            // Decrypt the ciphertext.
            byte[] plainText = cipher.doFinal(cipherText);
            return new String(plainText);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (NoSuchProviderException e) {
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            e.printStackTrace();
        } catch (BadPaddingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```