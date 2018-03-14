# Install and Use the AWS CloudHSM Software Library for PKCS \#11<a name="pkcs11-library-install"></a>

AWS CloudHSM provides two software libraries for PKCS \#11\. One uses Redis to create a local cache for efficiency, which can increase performance\. However, consider the following before you choose the library with Redis:

**Considerations**  
Redis caches all operations performed with the PKCS \#11 library running on the same host, but it's not aware of operations that are performed outside the library\. You can use another interface to modify keys on the HSMs in your cluster—for example, the [command line tools](command-line-tools.md) or [software library for Java](java-library.md)\. But if you do, the Redis cache can fall out of sync with the HSMs\. You can rebuild the cache to bring it back into sync, but it doesn't happen automatically\.
The PKCS \#11 library expects that it's the only Redis consumer on the host, and it modifies some of the Redis configuration accordingly\. Don't use the PKCS \#11 library with Redis when you have other applications that use Redis on the same host\.


+ [Prerequisites](#pkcs11-library-prerequisites)
+ [Install the PKCS \#11 Library](#install-pkcs11-library)
+ [Specify a PIN for PKCS \#11](#specify-pin-pkcs11)

## Prerequisites<a name="pkcs11-library-prerequisites"></a>

Before you can use the AWS CloudHSM software library for PKCS \#11, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster, and the PKCS \#11 library communicates locally with the client\. If you haven't installed and configured the AWS CloudHSM client package, do that now by following the steps at [Install the CloudHSM Client](install-and-configure-client.md)\. After you install and configure the client, use the following command to start it\.

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

## Install the PKCS \#11 Library<a name="install-pkcs11-library"></a>

Complete the following steps to install or update the AWS CloudHSM software library for PKCS \#11\.

Use the following commands to download and install the PKCS \#11 library\.

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11-latest.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.x86_64.rpm
```

------
#### [ Ubuntu ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-pkcs11_latest_amd64.deb
```

```
$ sudo dpkg -i cloudhsm-client-pkcs11_latest_amd64.deb
```

------

After you complete the preceding steps, you can find the PKCS \#11 libraries in `/opt/cloudhsm/lib`\.

**To install \(or update\) the PKCS \#11 library with Redis**

1. Complete the preceding steps to install the PKCS \#11 library\.

1. Complete the following steps to enable the repository named Extra Packages for Enterprise Linux\.

   1. Use a text editor to open the file `/etc/yum.repos.d/epel.repo`\. This requires administrative permissions \(`sudo`\)\.

   1. In the `[epel]` configuration, ensure that `enabled` is set to `1`, as shown in the following example\.

      ```
      [epel]
      name=Extra Packages for Enterprise Linux 6 - $basearch
      #baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
      mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
      failovermethod=priority
      enabled=1
      gpgcheck=1
      gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
      ```

   1. Save the file, and then close it\.

1. Use the following command to change your working directory to `/opt/cloudhsm`\.

   ```
   $ cd /opt/cloudhsm
   ```

1. Use the following command to install Redis and configure it for the PKCS \#11 library\.

   ```
   $ sudo /opt/cloudhsm/bin/setup_redis
   ```

1. Use the following command to start the Redis service\.

   ```
   $ sudo service redis start
   ```

1. Use the following command to build the Redis cache, specifying the user name and password of a crypto user \(CU\) on the HSM\.

   ```
   $ /opt/cloudhsm/bin/build_keystore -s <CU user name> -p <password>
   ```

## Specify a PIN for PKCS \#11<a name="specify-pin-pkcs11"></a>

The PKCS \#11 interface defines a PIN \(personal identification number\) for users of a cryptographic token\. To specify a PKCS \#11 PIN in the context of the AWS CloudHSM software library for PKCS\#11, use the following format\.

```
<HSM_user_name>:<password>
```

For example, the following is the PKCS \#11 PIN for an HSM [crypto user \(CU\)](hsm-users.md) with user name `CryptoUser` and password `CUPassword123!`\.

```
CryptoUser:CUPassword123!
```