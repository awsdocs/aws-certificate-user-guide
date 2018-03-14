# Managing Keys in an HSM<a name="java-sample-manage-keys"></a>


|  | 
| --- |
|  \*\* Example code only \- Not for production use \*\* This page includes example code that has not been fully tested\. It is designed for test environments\. Do not run this code in production\.  | 

This sample shows how to manage keys in an HSM\. It demonstrates the following operations:

+ **Get** a reference to a key in the HSM\.

+ **Export** a key from the HSM\. This operation returns the key so that you can import it into a different HSM and use it in other operations\. It does not delete the key from the HSM\.

+ **Delete** a key from the HSM\.

+ **Import** a key into the HSM\. This example returns a key handle that you can use to identify the key in other operations\.

**Note**  
To use a key in an encryption operation, specify the key handle\. You do not need to get or export the key\.

This sample uses the `loginWithExplicitCredentials()` method in the [Log In To and Out Of an HSM](java-sample-login.md) sample to log in to the HSM, but you can substitute the login method that you prefer\. Also, the example assumes that [the Cavium provider](use-cavium-provider.md) is included in your Java provider file\. If it is not, create an instance of the provider and substitute it for the **Cavium** string\. 

You can also use the **key\_mgmt\_util** command line tool to [manage keys in AWS CloudHSM](manage-keys.md)\. 

```
package com.amazonaws.cloudhsm.examples;

import java.security.InvalidKeyException;
import java.security.Key;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;
import java.util.Vector;

import javax.crypto.BadPaddingException;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import javax.crypto.KeyGenerator;

import org.bouncycastle.util.Arrays;

import com.cavium.cfm2.CFM2Exception;
import com.cavium.cfm2.ImportKey;
import com.cavium.cfm2.Util;
import com.cavium.key.CaviumAESKey;
import com.cavium.key.CaviumKey;
import com.cavium.key.CaviumKeyAttributes;
import com.cavium.key.CaviumRSAPrivateKey;
import com.cavium.key.CaviumRSAPublicKey;
import com.cavium.key.parameter.CaviumKeyGenAlgorithmParameterSpec;

public class KeyManagement {

    public static void main(String[] args) {
        LoginLogoutExample.loginWithExplicitCredentials();
        
        // Get a reference to a key in the HSM.
        // Replace the placeholder with an actual key handle value.
        long keyHandle =262194;    
        CaviumKey ck = getKey(keyHandle);
        
        // Delete the specified key from the HSM.
        // Replace the placeholder with an actual key handle value.
        deleteKey(51);
        Key key = exportKey(keyHandle);
        
        // Import a key.
        // Generate a 256-bit AES symmetric key.
        KeyGenerator kg = KeyGenerator.getInstance("AES");
        kg.init(256);
        Key keyToBeImported = kg.generateKey();
        
        // Import the key as extractable and persistent.
        // You can use the key handle to identify the key in other operations.
        long importedKeyHandle = importKey(keyToBeImported, "Test", true, true);
        System.out.println("Imported Key Handle : " + importedKeyHandle);

        LoginLogoutExample.logout();
    }
    
    // Get an existing key from the HSM.
    // The type of the object that is returned depends on the key type.
    public static CaviumKey getKey(long handle) {
        try {
            byte[] keyAttribute = Util.getKeyAttributes(handle);
            CaviumKeyAttributes cka = new CaviumKeyAttributes(keyAttribute);
            if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_AES) {
                CaviumAESKey aesKey = new CaviumAESKey(handle, cka);
                return aesKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PRIVATE_KEY) {
                CaviumRSAPrivateKey privKey = new CaviumRSAPrivateKey(handle, cka);
                return privKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PUBLIC_KEY) {
                CaviumRSAPublicKey pubKey = new CaviumRSAPublicKey(handle, cka);
                return pubKey;
            }
        } catch (CFM2Exception e) {
            e.printStackTrace();
        }
        return null;        
    }
    
    // Delete an existing persisted key.
    public static void deteleKey(long handle) {
        CaviumKey ck = getKey(handle);
        try {
            Util.deleteKey(ck);
            System.out.println("Key Deleted!");
        } catch (CFM2Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
    
    // Export an existing persisted key.
    // The type of the object that is returned depends on the key type.
    public static Key exportKey(long handle) {
        try {
            byte[] encoded = Util.exportKey( handle);
            byte[] keyAttribute = Util.getKeyAttributes(handle);
            CaviumKeyAttributes cka = new CaviumKeyAttributes(keyAttribute);
            if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_AES) {
                Key aesKey = new SecretKeySpec(encoded, 0, encoded.length, "AES");
                return aesKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PRIVATE_KEY) {
                PrivateKey privateKey = KeyFactory.getInstance("RSA").generatePrivate(new PKCS8EncodedKeySpec(encoded));
                return privateKey;
            }
            else if(cka.getKeyType() == CaviumKeyAttributes.KEY_TYPE_RSA && cka.getKeyClass() == CaviumKeyAttributes.CLASS_PUBLIC_KEY) {
                PublicKey publicKey = KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(encoded));
                return publicKey;
            }
            System.out.println(new String(encoded));
        } catch (BadPaddingException | CFM2Exception e) {
            e.printStackTrace();
        } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } 
        return null;
    }

    // Import a key.
    public static void importKey(Key key, String keyLabel, boolean isExtractable, boolean isPersistent) {
    
        // Create a new key parameter spec to identify the key. Specify a label 
        // and Boolean values for extractable and persistent.
        CaviumKeyGenAlgorithmParameterSpec spec = new CaviumKeyGenAlgorithmParameterSpec(keyLabel, isExtractable, isPersistent);
        try {
            ImportKey.importKey(key, spec);
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        }
    }
    
}
```