# Encrypting and Decrypting with AES GCM<a name="java-sample-symmetric-encrypt-decrypt"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

The example shows how to use the AES algorithm with the Galois Counter Mode \(GCM\) to encrypt and decrypt a message\. The example uses an initialization vector \(IV\), additional authenticated data \(AAD\), and no padding\. The `encrypt()` method returns an object that includes the ciphertext, the IV, the length of the ciphertext, and the length of the IV\. 

**Note**  
This example uses the `loginWithExplicitCredentials()` method in the [Log In To and Out Of an HSM](java-sample-login.md) sample to log in to the HSM\. This sample uses the AESSymmetricKeyGeneration discussed in the [Create an AES Key](java-sample-aes-key.md)\. The sample assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

```
package com.amazonaws.cloudhsm.examples.crypto.symmetric;

import java.security.InvalidAlgorithmParameterException;
import java.security.InvalidKeyException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.util.Arrays;
import java.util.Base64;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.spec.GCMParameterSpec;

import com.amazonaws.cloudhsm.examples.key.symmetric.AESSymmetricKeyGeneration;
import com.amazonaws.cloudhsm.examples.operations.LoginLogoutExample;
import com.cavium.key.CaviumAESKey;

public class AESGCMEncryptDecryptExample {

  String plainText = "This!";                    // String to encrypt.
  String aad = "aad";                            // Additional authenticated data
  String transformation = "AES/GCM/NoPadding";   // No padding
  
  int tagLengthInBytes = 16;                     // Tag length
  int ivSizeReturnedByHSM;                       // Size of IV returned by HSM
  public static void main(String[] z) {

    AESGCMEncryptDecryptExample obj = new AESGCMEncryptDecryptExample();
    
    // Log in to HSM with cryptographic user permissions.
    LoginLogoutExample.loginWithExplicitCredentials();

    // Generate a new AES Key to use for encryption.
    Key key =new AESSymmetricKeyGeneration().generateAESKey(256, true);
    byte[] iv = null;

    // Encrypt the plaintext.
    System.out.println("Performing AES Encryption Operation");
    byte[] result = obj.encrypt(obj.transformation, (CaviumAESKey) key, obj.plainText, obj.aad, obj.tagLengthInBytes);
    System.out.println("Plaintext Encrypted");

    System.out.println("Base64 Encoded Encrypted Text with IV = " + Base64.getEncoder().encodeToString(result));

    System.out.println("Perfroming Decrypt Operation");
    
    // Extract the IV created by the HSM during encryption.
    iv = Arrays.copyOfRange(result, 0, obj.ivSizeReturnedByHSM);

    // Get the ciphertext.
    byte[] cipherText = Arrays.copyOfRange(result, obj.ivSizeReturnedByHSM, result.length);
    System.out.println("Base64 Encoded Cipher Text = " + Base64.getEncoder().encodeToString(cipherText));

    // Decrypt the ciphertext.
    byte[] decryptedText = obj.decrypt(obj.transformation, (CaviumAESKey) key, cipherText, iv, obj.aad, obj.tagLengthInBytes);
    System.out.println("Plain Text = "+new String(decryptedText));
    LoginLogoutExample.logout();
  }

  // Encrypt the plaintext.
  public byte[] encrypt(String transformation, CaviumAESKey key, String plainText, String aad, int tagLength) {
    try {
      // Create an encryption cipher.
      Cipher encCipher = Cipher.getInstance(transformation, "Cavium");

      // Configure the encryption cipher.
      encCipher.init(Cipher.ENCRYPT_MODE, key);
      encCipher.updateAAD(aad.getBytes());
      encCipher.update(plainText.getBytes());
      
      // Encrypt the plaintext data.
      byte[] ciphertext = encCipher.doFinal();

      // You'll get a new IV from the HSM after encryption. Save it. You'll need it to recreate the 
      // GCM parameter spec for decryption. The IV returned by the HSM has a fixed length 16 bytes. 
      // Append the IV to the ciphertext for easier management.
      ivSizeReturnedByHSM = encCipher.getIV().length;
      byte[] finalResult = new byte[ivSizeReturnedByHSM + ciphertext.length]; 
      System.arraycopy(encCipher.getIV(), 0, finalResult, 0, ivSizeReturnedByHSM);
      System.arraycopy(ciphertext, 0, finalResult, ivSizeReturnedByHSM, ciphertext.length);
      return finalResult;

    } catch (NoSuchAlgorithmException | NoSuchProviderException | NoSuchPaddingException e) {
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

  // Decrypt the ciphertext using the the specified algorithm, key, IV, and AAD.
  public byte[] decrypt(String transformation, CaviumAESKey key, byte[] cipherText , byte[] iv, String aad, int tagLength) {
    Cipher decCipher;
    try {
      // Create the decryption cipher.
      decCipher = Cipher.getInstance(transformation, "Cavium");
      
      // Create a GCM parameter spec from the IV.
      GCMParameterSpec gcmSpec = new GCMParameterSpec(tagLengthInBytes * Byte.SIZE,iv);
      
      // Configure the decryption cipher.
      decCipher.init(Cipher.DECRYPT_MODE, key, gcmSpec);
      decCipher.updateAAD(aad.getBytes());
      
      // Decrypt the ciphertext and return the plaintext.
      return decCipher.doFinal(cipherText);

    } catch (NoSuchAlgorithmException | NoSuchProviderException | NoSuchPaddingException e) {
      e.printStackTrace();
    } catch (InvalidKeyException e) {
      e.printStackTrace();
    } catch (InvalidAlgorithmParameterException e) {
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