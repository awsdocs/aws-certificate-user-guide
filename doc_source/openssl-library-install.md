# Install and Use the AWS CloudHSM Software Library for OpenSSL<a name="openssl-library-install"></a>

Before you can use the AWS CloudHSM software library for OpenSSL, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster, and the OpenSSL library communicates locally with the client\. If you haven't installed and configured the AWS CloudHSM client package, do that now by following the steps at [Install the CloudHSM Client](install-and-configure-client.md)\. After you install and configure the client, use the following command to start it\.

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


+ [Install and Configure the OpenSSL Library](#install-openssl-library)
+ [Use the OpenSSL Library](#use-openssl-library)

## Install and Configure the OpenSSL Library<a name="install-openssl-library"></a>

Complete the following steps to install \(or update\) and configure the AWS CloudHSM software library for OpenSSL\.

**To install \(or update\) and configure the OpenSSL library**

1. Use the following command to download the OpenSSL library\.

------
#### [ Amazon Linux ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn-latest.x86_64.rpm
   ```

------
#### [ Ubuntu ]

   ```
   $ wget https://s3.amazonaws.com/cloudhsmv2-software/cloudhsm-client-dyn_latest_amd64.deb
   ```

------

1. Use the following command to install the OpenSSL library\.

------
#### [ Amazon Linux ]

   ```
   $ sudo yum install -y ./cloudhsm-client-dyn-latest.x86_64.rpm
   ```

------
#### [ Ubuntu ]

   ```
   $ sudo dpkg -i cloudhsm-client-dyn_latest_amd64.deb
   ```

------

   After you complete the preceding step, you can find the OpenSSL library at `/opt/cloudhsm/lib/libcloudhsm_openssl.so`\.

1. Use the following command to set an environment variable named `n3fips_password` that contains the credentials of a crypto user \(CU\)\.

   ```
   $ export n3fips_password=<HSM user name>:<password>
   ```

## Use the OpenSSL Library<a name="use-openssl-library"></a>

To use the AWS CloudHSM software library for OpenSSL from the OpenSSL command line, use the `-engine` option to specify the OpenSSL dynamic engine named `cloudhsm`\. For example:

```
$ openssl s_server -cert server.crt -key server.key -engine cloudhsm
```

To use the AWS CloudHSM software library for OpenSSL from an OpenSSL\-integrated application, ensure that your application uses the OpenSSL dynamic engine named `cloudhsm`\. The shared library for the dynamic engine is located at `/opt/cloudhsm/lib/libcloudhsm_openssl.so`\.