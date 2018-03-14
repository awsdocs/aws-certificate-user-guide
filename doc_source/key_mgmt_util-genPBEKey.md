# genPBEKey<a name="key_mgmt_util-genPBEKey"></a>

The `genPBE` command in the key\_mgmt\_util tool generates a Triple DES \(3DES\) symmetric key based on a password\. This command is not supported on the FIPS\-validated HSMs that AWS CloudHSM provides\. 

To create symmetric keys, use [genSymKey](key_mgmt_util-genSymKey.md)\. To create asymmetric key pairs, use [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md), [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md), or [genECCKeyPair](key_mgmt_util-genECCKeyPair.md)\.