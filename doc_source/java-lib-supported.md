# Supported Mechanisms<a name="java-lib-supported"></a>

For information about the Java Cryptography Architecture \(JCA\) interfaces and engine classes supported by AWS CloudHSM, see the following topics\. 


+ [Supported Keys](#java-keys)
+ [Supported Ciphers](#java-ciphers)
+ [Supported Digests](#java-digests)
+ [Supported Hash\-Based Message Authentication Code \(HMAC\) Algorithms](#java-mac)
+ [Supported Sign/Verify Mechanisms](#java-sign-verify)

## Supported Keys<a name="java-keys"></a>

The AWS CloudHSM software library for Java enables you to generate the following key types\.

+ **RSA** – 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\.

+ **AES** – 128, 192, and 256\-bit AES keys\.

+ EC key pairs for NIST curves P256 and P384\.

In addition to standard parameters, we support the following parameters for each key that is generated\.

+ **Label**: A key label that you can use to search for keys\.

+ **isExtractable**: Indicates whether the key can be exported from the HSM\. \(Imported keys are never extractable\.\)

+ **isPersistent**: Indicates whether the key remains on the HSM when the current session ends\.

## Supported Ciphers<a name="java-ciphers"></a>

The AWS CloudHSM software library for Java supports the following algorithm, mode, and padding combinations\.


| Algorithm | Mode | Padding | Notes | 
| --- | --- | --- | --- | 
| AES | CBC |  `AES/CBC/NoPadding` `AES/CBC/PKCS5Padding`  | Implements Cipher\.ENCRYPT\_MODE, Cipher\.DECRYPT\_MODE, Cipher\.WRAP\_MODE, and Cipher\.UNWRAP\_MODE\. | 
| AES | GCM | AES/GCM/NoPadding | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.When performing AES\-GCM encryption, the HSM ignores the initialization vector \(IV\) in the request and uses an IV that it generates\. When the operation completes, you must call `Cipher.getIV()` to get the IV\. | 
| DESede \(Triple DES\) | CBC |  `DESede/CBC/NoPadding` `DESede/CBC/PKCS5Padding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\. The key generation routines accept a size of 168 or 192 bits\. However, internally, all DESede keys are 192 bits\.  | 
| RSA | ECB | `RSA/ECB/NoPadding``RSA/ECB/PKCS1Padding` | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\. | 
| RSA | ECB | `RSA/ECB/OAEPPadding` `RSA/ECB/OAEPWithSHA-1ANDMGF1Padding` `RSA/ECB/PKCS1Padding` `RSA/ECB/OAEPPadding` `RSA/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-512ANDMGF1Padding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\. `OAEPPadding` is `OAEP` with the `SHA-1` padding type\.  | 

## Supported Digests<a name="java-digests"></a>

The AWS CloudHSM software library for Java supports the following message digests\.

+ `SHA-1`

+ `SHA-224`

+ `SHA-256`

+ `SHA-384`

+ `SHA-512`

## Supported Hash\-Based Message Authentication Code \(HMAC\) Algorithms<a name="java-mac"></a>

The AWS CloudHSM software library for Java supports the following HMAC algorithms\.

+ `HmacSHA1`

+ `HmacSHA224`

+ `HmacSHA256`

+ `HmacSHA384`

+ `HmacSHA512`

## Supported Sign/Verify Mechanisms<a name="java-sign-verify"></a>

The AWS CloudHSM software library for Java supports the following types of signature and verification\.

**RSA Signature Types**

+ `NONEwithRSA`

+ `SHA1withRSA`

+ `SHA224withRSA`

+ `SHA256withRSA`

+ `SHA384withRSA`

+ `SHA512withRSA`

+ `SHA1withRSA/PSS`

+ `SHA224withRSA/PSS`

+ `SHA256withRSA/PSS`

+ `SHA384withRSA/PSS`

+ `SHA512withRSA/PSS`

**ECDSA Signature Types**

+ `NONEwithECDSA`

+ `SHA1withECDSA`

+ `SHA224withECDSA`

+ `SHA256withECDSA`

+ `SHA384withECDSA`

+ `SHA512withECDSA`