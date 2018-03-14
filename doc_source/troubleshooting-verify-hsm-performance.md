# Verify the Performance of the HSM<a name="troubleshooting-verify-hsm-performance"></a>

To verify the performance of the HSMs in your AWS CloudHSM cluster, you can use the pkpspeed tool that is included with the AWS CloudHSM client software\. To use pkpspeed, first connect to your client instance and then [install and configure the AWS CloudHSM client software](install-and-configure-client.md)\. 

After you install and configure the AWS CloudHSM client, issue the following command to start it\.

------
#### [ Amazon Linux ]

```
$ sudo start cloudhsm-client
```

------
#### [ Ubuntu ]

```
$ sudo service cloudhsm-client start
```

------

If you have already installed the client software, you might need to download and install the latest version to get pkpspeed\. You can find the pkpspeed tool at `/opt/cloudhsm/bin/pkpspeed`\. 

To use pkpspeed, issue the pkpspeed command, specifying the user name and password of a crypto user \(CU\) on the HSM\. Then set the options to use, considering the following recommendations\. 

**Recommendations**  
To test the performance of RSA sign and verify operations, choose the `RSA_CRT` cipher\. Don't choose `RSA`\. The ciphers are equivalent, but `RSA_CRT` is optimized for performance\. 
Start with a small number of threads\. For testing AES performance, one thread is typically enough to show maximum performance\. For testing RSA performance\(`RSA_CRT`\), three or four threads is typically enough\. 

The following examples show the options that you can choose with pkpspeed to test the HSM's performance for RSA and AES operations\. 

**Example – Using pkpspeed to test RSA performance**  

```
/opt/cloudhsm/bin/pkpspeed -s <CU user name> -p <password>
	
SDK Version: 2.03

        Available Ciphers:
                AES_128
                AES_256
                3DES
                RSA  (non-CRT. modulus size can be 2048/3072)
                RSA_CRT (same as RSA)
For RSA, Exponent will be 65537

Current FIPS mode is: 00000002
Enter the number of thread [1-10]: 3
Enter the cipher: RSA_CRT
Enter modulus length: 2048
Enter time duration in Secs: 60
Starting non-blocking speed test using data length of 245 bytes...
[Test duration is 60 seconds]

Do you want to use static key[y/n] (Make sure that KEK is available)?n
```

**Example – Using pkpspeed to test AES performance**  

```
/opt/cloudhsm/bin/pkpspeed -s <CU user name> -p <password>

SDK Version: 2.03

        Available Ciphers:
                AES_128
                AES_256
                3DES
                RSA  (non-CRT. modulus size can be 2048/3072)
                RSA_CRT (same as RSA)
For RSA, Exponent will be 65537

Current FIPS mode is: 00000002
Enter the number of thread [1-10]: 1
Enter the cipher: AES_256
Enter the data size [1-16200]: 8192
Enter time duration in Secs: 60
Starting non-blocking speed test using data length of 8192 bytes...
```