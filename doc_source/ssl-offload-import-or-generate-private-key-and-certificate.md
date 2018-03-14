# Web Server SSL/TLS Offload Step 2: Import or Generate a Private Key and SSL/TLS Certificate<a name="ssl-offload-import-or-generate-private-key-and-certificate"></a>

To enable HTTPS, your web server application \(Nginx or Apache\) needs a private key and a corresponding SSL/TLS certificate\. To use web server SSL/TLS offload with AWS CloudHSM, you must store the private key in an HSM in your AWS CloudHSM cluster\. You can accomplish this in one of the following ways: 

+ If you don't yet have a private key and a corresponding certificate, you can [generate a private key in an HSM](#ssl-offload-generate-private-key-and-certificate)\. You can then use the private key to create a certificate signing request \(CSR\)\. Use the CSR to create the SSL/TLS certificate\. 

   

+ If you already have a private key and corresponding certificate, you can [import the private key into an HSM](#ssl-offload-import-private-key)\. 

Regardless of which method you choose, you then export a *fake PEM private key* from the HSM and save it to a file\. This file doesn't contain the actual private key\. It contains a reference to the private key that is stored on the HSM\. Your web server uses the fake PEM private key file and the AWS CloudHSM software library for OpenSSL to offload SSL/TLS processing to an HSM\. 

## Generate a Private Key and Certificate<a name="ssl-offload-generate-private-key-and-certificate"></a>

If you don't have a private key and a corresponding SSL/TLS certificate to use for HTTPS, you can generate a private key on an HSM\. You can then you use the private key to create a certificate signing request \(CSR\)\. Sign the CSR to create the certificate\. <a name="ssl-offload-generate-private-key-steps"></a>

**To generate a private key on an HSM**

1. Connect to your client instance\.

1. Run the following command to set an environment variable named `n3fips_password` that contains the user name and password of the cryptographic user \(CU\)\. Replace *<CU user name>* with the user name of the cryptographic user\. Replace *<password>* with the CU password\. 

   ```
   export n3fips_password=<CU user name>:<password>
   ```

1. Run the following command to use the AWS CloudHSM software library for OpenSSL to generate a private key on an HSM\. This command also exports the fake PEM private key and saves it in a file\. Replace *<web\_server\_fake\_PEM\.key>* with the file name you want to use for the exported fake PEM private key\. 

   ```
   openssl genrsa -engine cloudhsm -out <web_server_fake_PEM.key> 2048
   ```

**To create a CSR**

Run the following command to use the AWS CloudHSM software library for OpenSSL to create a certificate signing request \(CSR\)\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\. Replace *<web\_server\.csr>* with the name of the file that contains your CSR\. 

The `req` command is interactive\. Respond to each field\. The field information is copied into your SSL/TLS certificate\. 

```
openssl req -engine cloudhsm -new -key <web_server_fake_PEM.key> -out <web_server.csr>
```

In a production environment, you typically use a certificate authority \(CA\) to create a certificate from a CSR\. A CA is not necessary for a test environment\. If you do use a CA, send the CSR file \(*<web\_server\.csr>*\) to it and use the CA create a signed SSL/TLS certificate\. Your web server uses the signed certificate for HTTPS\. 

As an alternative to using a CA, you can use the AWS CloudHSM software library for OpenSSL to create a self\-signed certificate\. Self\-signed certificates are not trusted by browsers and should not be used in production environments\. They can be used in test environments\. 

**To create a self\-signed certificate**

**Important**  
Self\-signed certificates should be used in a test environment only\. For a production environment, use a more secure method such as a certificate authority to create a certificate\. 

Run the following command to use the AWS CloudHSM software library for OpenSSL to sign your CSR with your private key on your HSM\. This creates a self\-signed certificate\. Replace the following values in the command with your own\. 

+ *<web\_server\.csr>* – Name of the file that contains the CSR\.

+ *<web\_server\_fake\_PEM\.key>* – Name of the file that contains the fake PEM private key\.

+ *<web\_server\.crt>* – Name of the file that will contain your web server\.

```
openssl x509 -engine cloudhsm -req -days 365 -in <web_server.csr> -signkey <web_server_fake_PEM.key> -out <web_server.crt>
```

After you complete these steps, you can [configure your web server](ssl-offload-configure-web-server.md)\. 

## Import an Existing Private Key<a name="ssl-offload-import-private-key"></a>

If you already have a private key and a corresponding SSL/TLS certificate that you use for HTTPS on your web server, you can import that key into an HSM by doing the following\. 

1. Connect to your Amazon EC2 client instance\. If necessary, copy your existing private key and certificate to the instance\. 

1. Run the following command to start the AWS CloudHSM client\.

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

1. Run the following command to start the key\_mgmt\_util command line tool\.

   ```
   /opt/cloudhsm/bin/key_mgmt_util
   ```

1. Run the following command to log in to the HSM\. Replace *<user name>* and *<password>* with the user name and password of the cryptographic user \(CU\)\. 

   ```
   loginHSM -u CU -s <user name> -p <password>
   ```

1. Run the following commands to import your private key into an HSM\.

   1. Run the following command to create a symmetric wrapping key that is valid for the current session only\. The command and output are shown\. 

      ```
      genSymKey -t 31 -s 16 -sess -l wrapping_key_for_import
      
      Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS
      Symmetric Key Created.  Key Handle: 6
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

   1. Run the following command to import your existing private key into an HSM\. The command and output are shown\. Replace the following values with your own: 

      + *<web\_server\_existing\.key>* – Name of the file that contains your private key\.

      + *<web\_server\_imported\_key>* – Label for your imported private key\.

      + *<wrapping\_key\_handle>* – Wrapping key handle generated by the preceding command\. In the previous example, the wrapping key handle is 6\.

      ```
      importPrivateKey -f <web_server_existing.key> -l <web_server_imported_key> -w <wrapping_key_handle>
      
      BER encoded key length is 1219
      Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
      Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS
      Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS
      Private Key Unwrapped.  Key Handle: 8
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

1. Run the following command to export the private key in fake PEM format and save it to a file\. Replace the following values with your own\. 

   + *<private\_key\_handle>* – Handle of the imported private key\. This handle was generated by the second command in the preceding step\. In the preceding example, the handle of the private key is 8\. 

   + *<web\_server\_fake\_PEM\.key>* – Name of the file that contains your exported fake PEM private key\. 

   ```
   getCaviumPrivKey -k <private_key_handle> -out <web_server_fake_PEM.key>
   ```

1. Run the following command to stop key\_mgmt\_util\.

   ```
   exit
   ```

After you complete these steps, go to [Configure the Web Server](ssl-offload-configure-web-server.md)\.