# AWS CloudHSM Cluster Backups<a name="backups"></a>

AWS CloudHSM makes periodic backups of your cluster\. You can't instruct AWS CloudHSM to make backups anytime that you want, but you can take certain actions that result in AWS CloudHSM making a backup\. For more information, see the following topics\.

When you add an HSM to a cluster that previously contained one or more active HSMs, AWS CloudHSM restores the most recent backup onto the new HSM\. This means that you can use AWS CloudHSM to manage an HSM that you use infrequently\. When you don't need to use the HSM, you can delete it, which triggers a backup\. Later, when you need to use the HSM again, you can create a new HSM in the same cluster, effectively restoring your previous HSM\.

You can also create a new cluster from an existing backup of a different cluster\. You must create the new cluster in the same AWS Region that contains the existing backup\.


+ [Overview of Backups](#backup-overview)
+ [Security of Backups](#backup-security)
+ [Durability of Backups](#backups-durability)
+ [Frequency of Backups](#backups-frequency)

## Overview of Backups<a name="backup-overview"></a>

Each backup contains encrypted copies of the following data:

+ All [users \(COs, CUs, and AUs\)](hsm-users.md) on the HSM\.

+ All key material and certificates on the HSM\.

+ The HSM's configuration and policies\.

AWS CloudHSM stores the backups in a service\-controlled Amazon Simple Storage Service \(Amazon S3\) bucket in the same AWS Region as your cluster\.

![\[AWS CloudHSM cluster backups encrypted in a service-controlled Amazon S3 bucket.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cluster-backup.png)

## Security of Backups<a name="backup-security"></a>

When AWS CloudHSM makes a backup from the HSM, the HSM encrypts all of its data before sending it to AWS CloudHSM\. The data never leaves the HSM in plaintext form\.

To encrypt its data, the HSM uses a unique, ephemeral encryption key known as the ephemeral backup key \(EBK\)\. The EBK is an AES 256\-bit encryption key generated inside the HSM when AWS CloudHSM makes a backup\. The HSM generates the EBK, then uses it to encrypt the HSM's data with a FIPS\-approved AES key wrapping method that complies with [NIST special publication 800\-38F](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38F.pdf)\. Then the HSM gives the encrypted data to AWS CloudHSM\. The encrypted data includes an encrypted copy of the EBK\.

To encrypt the EBK, the HSM uses another encryption key known as the persistent backup key \(PBK\)\. The PBK is also an AES 256\-bit encryption key\. To generate the PBK, the HSM uses a FIPS\-approved key derivation function \(KDF\) in counter mode that complies with [NIST special publication 800\-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)\. The inputs to this KDF include the following:

+ A manufacturer key backup key \(MKBK\), permanently embedded in the HSM hardware by the hardware manufacturer\.

+ An AWS key backup key \(AKBK\), securely installed in the HSM when it's initially configured by AWS CloudHSM\.

The encryption processes are summarized in the following figure\. The backup encryption key represents the persistent backup key \(PBK\) and the ephemeral backup key \(EBK\)\. 

![\[A summary of the encryption keys that are used to encrypt AWS CloudHSM backups.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/backup-security.png)

AWS CloudHSM can restore backups onto only AWS\-owned HSMs made by the same manufacturer\. Because each backup contains all users, keys, and configuration from the original HSM, the restored HSM contains the same protections and access controls as the original\. The restored data overwrites all other data that might have been on the HSM prior to restoration\.

A backup consists of only encrypted data\. Before each backup is stored in Amazon S3, it's encrypted again under an AWS Key Management Service \(AWS KMS\) customer master key \(CMK\)\.

## Durability of Backups<a name="backups-durability"></a>

AWS CloudHSM stores cluster backups in an Amazon S3 bucket in an AWS account that AWS CloudHSM controls\. The durability of backups is the same as any object stored in Amazon S3\. Amazon S3 is designed to deliver 99\.999999999% durability\.

## Frequency of Backups<a name="backups-frequency"></a>

AWS CloudHSM makes a cluster backup at least once per 24 hours\. In addition to recurring daily backups, AWS CloudHSM makes a backup when you perform any of the following actions:

+ [Initialize the cluster](initialize-cluster.md)\.

+ [Add an HSM to an initialized cluster](add-remove-hsm.md#add-hsm)\.

+ [Remove an HSM from a cluster](add-remove-hsm.md#remove-hsm)\.