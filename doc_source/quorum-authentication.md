# Enforcing Quorum Authentication \(M of N Access Control\)<a name="quorum-authentication"></a>

The HSMs in your AWS CloudHSM cluster support quorum authentication, which is also known as M of N access control\. With quorum authentication, no single user on the HSM can do quorum\-controlled operations on the HSM\. Instead, a minimum number of HSM users \(at least 2\) must cooperate to do these operations\. With quorum authentication, you can add an extra layer of protection by requiring approvals from more than one HSM user\.

Quorum authentication can control the following operations:

+ HSM user management by [crypto officers \(COs\)](hsm-users.md#crypto-officer) – Creating and deleting HSM users, and changing a different HSM user's password\. For more information, see [Using Quorum Authentication for Crypto Officers](quorum-authentication-crypto-officers.md)\.

+ Cryptographic operations by [crypto users \(CUs\)](hsm-users.md#crypto-user) – For example:

  + Using asymmetric private keys on the HSM to cryptographically sign messages\.

  + Using AES symmetric keys on the HSM for AES wrap and unwrap\.

The following topics provide more information about quorum authentication in AWS CloudHSM\.


+ [Overview of Quorum Authentication](#quorum-authentication-overview)
+ [Additional Details about Quorum Authentication](#quorum-authentication-details)
+ [Using Quorum Authentication for Crypto Officers: First Time Setup](quorum-authentication-crypto-officers-first-time-setup.md)
+ [Using Quorum Authentication for Crypto Officers](quorum-authentication-crypto-officers.md)
+ [Change the Quorum Minimum Value for Crypto Officers](quorum-authentication-crypto-officers-change-minimum-value.md)

## Overview of Quorum Authentication<a name="quorum-authentication-overview"></a>

The following steps summarize the quorum authentication processes\. For the specific steps and tools, see [Using Quorum Authentication for Crypto Officers](quorum-authentication-crypto-officers.md)\.

1. Each HSM user creates an asymmetric key for signing\. He or she does this outside of the HSM, taking care to protect the key appropriately\.

1. Each HSM user logs in to the HSM and registers the public part of his or her signing key \(the public key\) with the HSM\.

1. When an HSM user wants to do a quorum\-controlled operation, he or she logs in to the HSM and gets a *quorum token*\.

1. The HSM user gives the quorum token to one or more other HSM users and asks for their approval\.

1. The other HSM users approve by using their keys to cryptographically sign the quorum token\. This occurs outside the HSM\.

1. When the HSM user has the required number of approvals, he or she logs in to the HSM and gives the quorum token and approvals \(signatures\) to the HSM\.

1. The HSM uses the registered public keys of each signer to verify the signatures\. If the signatures are valid, the HSM approves the token\.

1. The HSM user can now do a quorum\-controlled operation\.

## Additional Details about Quorum Authentication<a name="quorum-authentication-details"></a>

Note the following additional information about using quorum authentication in AWS CloudHSM\.

+ An HSM user can sign his or her own quorum token—that is, the requesting user can provide one of the required approvals for quorum authentication\.

+ You choose the minimum number of quorum approvers for quorum\-controlled operations\. The smallest number you can choose is two \(2\)\. For HSM user management operations by COs, the largest number you can choose is twenty \(20\)\. For cryptographic operations by CUs, the largest number you can choose is eight \(8\)\.

+ The HSM can store up to 1024 quorum tokens\. If the HSM already has 1024 tokens when you try to create a new one, the HSM purges one of the expired tokens\. By default, tokens expire ten minutes after their creation\.