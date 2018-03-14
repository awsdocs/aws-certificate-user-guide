# HSM Users<a name="hsm-users"></a>

Most operations that you perform on the HSM require the credentials of an *HSM user*\. The HSM authenticates each HSM user by means of a user name and password\.

Each HSM user has a *type* that determines which operations the user is allowed to perform on the HSM\. The following topics explain the types of HSM users\.


+ [Precrypto Officer \(PRECO\)](#preco)
+ [Crypto Officer \(CO, PCO\)](#crypto-officer)
+ [Crypto User \(CU\)](#crypto-user)
+ [Appliance User \(AU\)](#appliance-user)
+ [HSM User Permissions Table](#user-permissions-table)

## Precrypto Officer \(PRECO\)<a name="preco"></a>

The precrypto officer \(PRECO\) is a temporary user that exists only on the first HSM in an AWS CloudHSM cluster\. The first HSM in a new cluster contains a PRECO user with a default user name and password\. To [activate a cluster](activate-cluster.md), you log in to the HSM and change the PRECO user's password\. When you change the password, the PRECO user becomes a crypto officer \(PCO\)\. The PRECO user can only change its own password and perform read\-only operations on the HSM\.

## Crypto Officer \(CO, PCO\)<a name="crypto-officer"></a>

A crypto officer \(CO\) can perform user management operations\. For example, a CO can create and delete users and change user passwords\. For more information, see the [HSM User Permissions Table](#user-permissions-table)\.

When you [activate a new cluster](activate-cluster.md), the first user on an HSM changes from a [Precrypto Officer](#preco) \(PRECO\) to a primary Crypto Officer \(PCO\)\. The PCO is the first CO created on the HSM\. However, the PCO has the same permissions on the HSM as any other CO\. 

## Crypto User \(CU\)<a name="crypto-user"></a>

A crypto user \(CU\) can perform the following key management and cryptographic operations\.

+ **Key management** – Create, delete, share, import, and export cryptographic keys\.

+ **Cryptographic operations** – Use cryptographic keys for encryption, decryption, signing, verifying, and more\.

For more information, see the [HSM User Permissions Table](#user-permissions-table)\.

## Appliance User \(AU\)<a name="appliance-user"></a>

The appliance user \(AU\) can perform cloning and synchronization operations\. AWS CloudHSM uses the AU to synchronize the HSMs in an AWS CloudHSM cluster\. The AU exists on all HSMs provided by AWS CloudHSM, and has limited permissions\. For more information, see the [HSM User Permissions Table](#user-permissions-table)\.

AWS uses the AU to perform cloning and synchronization operations on your cluster's HSMs\. AWS cannot perform any operations on your HSMs except those granted to the AU and unauthenticated users\. AWS cannot view or modify your users or keys and cannot perform any cryptographic operations using those keys\.

## HSM User Permissions Table<a name="user-permissions-table"></a>

The following table lists HSM operations and whether each type of HSM user can perform them\.


|  | Crypto Officer \(CO\) | Crypto User \(CU\) | Appliance User \(AU\) | Unauthenticated user | 
| --- | --- | --- | --- | --- | 
| Get basic cluster info¹ | Yes | Yes | Yes | Yes | 
| Zeroize an HSM² | Yes | Yes | Yes | Yes | 
| Change own password | Yes | Yes | Yes | Not applicable | 
| Change any user's password | Yes | No | No | No | 
| Add, remove users | Yes | No | No | No | 
| Get sync status³ | Yes | Yes | Yes | No | 
| Extract, insert masked objects⁴ | Yes | Yes | Yes | No | 
| Create, share, delete keys | No | Yes | No | No | 
| Encrypt, decrypt | No | Yes | No | No | 
| Sign, verify | No | Yes | No | No | 
| Generate digests and HMACs | No | Yes | No | No | 

¹Basic cluster information includes the number of HSMs in the cluster and each HSM's IP address, model, serial number, device ID, firmware ID, etc\.

²When an HSM is zeroized, all keys, certificates, and other data on the HSM is destroyed\. You can use your cluster's security group to prevent an unauthenticated user from zeroizing your HSM\. For more information, see [Create a Cluster](create-cluster.md)\.

³The user can get a set of digests \(hashes\) that correspond to the keys on the HSM\. An application can compare these sets of digests to understand the synchronization status of HSMs in a cluster\.

⁴Masked objects are keys that are encrypted before they leave the HSM\. They cannot be decrypted outside of the HSM\. They are only decrypted after they are inserted into an HSM that is in the same cluster as the HSM from which they were extracted\. An application can extract and insert masked objects to synchronize the HSMs in a cluster\.