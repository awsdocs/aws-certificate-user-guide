# Web Server SSL/TLS Offload Step 3: Configure the Web Server<a name="ssl-offload-configure-web-server"></a>

To finish setting up your web server for SSL/TLS offload with AWS CloudHSM, see the following topics:


+ [Update the Web Server Configuration](#ssl-offload-update-web-server-configuration)
+ [Add the Web Server to a Security Group](#ssl-offload-add-security-group)
+ [Verify That HTTPS Uses the Private Key in Your AWS CloudHSM Cluster](#ssl-offload-verify-https-connection)

## Update the Web Server Configuration<a name="ssl-offload-update-web-server-configuration"></a>

To update your web server configuration, complete the steps in one of the following procedures\. Choose the procedure that corresponds to your web server software\. 

+ [Update the configuration for Nginx](#update-web-server-config-nginx)

+ [Update the configuration for Apache HTTP Server](#update-web-server-config-apache)<a name="update-web-server-config-nginx"></a>

**Update the web server configuration for Nginx**

1. Connect to your client instance\.

1. Run the following command to create the required directories for the web server certificate and the fake PEM private key\. 

   ```
   sudo mkdir -p /etc/pki/nginx/private
   ```

1. Run the following command to copy your web server certificate to the required location\. Replace *<web\_server\.crt>* with the name of your web server certificate\.

   ```
   sudo cp <web_server.crt> /etc/pki/nginx/server.crt
   ```

1. Run the following command to copy your fake PEM private key to the required location\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

   ```
   sudo cp <web_server_fake_PEM.key> /etc/pki/nginx/private/server.key
   ```

1. Run the following command to change the file ownership so that the user named *nginx* can read them\. 

   ```
   sudo chown nginx /etc/pki/nginx/server.crt /etc/pki/nginx/private/server.key
   ```

1. Run the following command to make a backup copy of the file named `/etc/nginx/nginx.conf`\.

   ```
   sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
   ```

1. Use a text editor to edit the file named `/etc/nginx/nginx.conf`\. At the top of the file, add the following line: 

   ```
   ssl_engine cloudhsm;
   ```

   Then uncomment the TLS section of the file so that it looks like the following:

   ```
   # Settings for a TLS enabled server.
   
       server {
           listen       443 ssl http2 default_server;
           listen       [::]:443 ssl http2 default_server;
           server_name  _;
           root         /usr/share/nginx/html;
   
           ssl_certificate "/etc/pki/nginx/server.crt";
           ssl_certificate_key "/etc/pki/nginx/private/server.key";
           # It is *strongly* recommended to generate unique DH parameters
           # Generate them with: openssl dhparam -out /etc/pki/nginx/dhparams.pem 2048
           #ssl_dhparam "/etc/pki/nginx/dhparams.pem";
           ssl_session_cache shared:SSL:1m;
           ssl_session_timeout  10m;
           ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
           ssl_ciphers HIGH:SEED:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!RSAPSK:!aDH:!aECDH:!EDH-DSS-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!SRP;
           ssl_prefer_server_ciphers on;
   
           # Load configuration files for the default server block.
           include /etc/nginx/default.d/*.conf;
   
           location / {
           }
   
           error_page 404 /404.html;
               location = /40x.html {
           }
   
           error_page 500 502 503 504 /50x.html;
               location = /50x.html {
           }
       }
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to make a backup copy of the file named `/etc/sysconfig/nginx`\.

   ```
   sudo cp /etc/sysconfig/nginx /etc/sysconfig/nginx.backup
   ```

1. Use a text editor to edit the file named `/etc/sysconfig/nginx`\. Add the following line, specifying the user name and password of the cryptographic user \(CU\)\. Replace *<CU user name>* with the user name of the cryptographic user\. Replace *<password>* with the CU password\. 

   ```
   export n3fips_password=<CU user name>:<password>
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to start the Nginx web server\.

   ```
   sudo service nginx start
   ```

1. Run the following command if you want to configure your server to start Nginx when the server starts\. 

   ```
   $ sudo chkconfig nginx on
   ```<a name="update-web-server-config-apache"></a>

**Update the web server configuration for Apache**

1. Connect to your Amazon EC2 client instance\.

1. Run the following command to make a backup copy of the default certificate\.

   ```
   sudo cp /etc/pki/tls/certs/localhost.crt /etc/pki/tls/certs/localhost.crt.backup
   ```

1. Run the following command to make a backup copy of the default private key\.

   ```
   sudo cp /etc/pki/tls/private/localhost.key /etc/pki/tls/private/localhost.key.backup
   ```

1. Run the following command to copy your web server certificate to the required location\. Replace *<web\_server\.crt>* with the name of your web server certificate\. 

   ```
   sudo cp <web_server.crt> /etc/pki/tls/certs/localhost.crt
   ```

1. Run the following command to copy your fake PEM private key to the required location\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\.

   ```
   sudo cp <web_server_fake_PEM.key> /etc/pki/tls/private/localhost.key
   ```

1. Run the following command to change the ownership of these files so that the user named *apache* can read them\. 

   ```
   sudo chown apache /etc/pki/tls/certs/localhost.crt /etc/pki/tls/private/localhost.key
   ```

1. Run the following command to make a backup copy of the file named `/etc/httpd/conf.d/ssl.conf`\.

   ```
   sudo cp /etc/httpd/conf.d/ssl.conf /etc/httpd/conf.d/ssl.conf.backup
   ```

1. Use a text editor to edit the file named `/etc/httpd/conf.d/ssl.conf`\. Replace the line that starts with `SSLCryptoDevice` so that it looks like the following: 

   ```
   SSLCryptoDevice cloudhsm
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to make a backup copy of the file named `/etc/sysconfig/httpd`\.

   ```
   sudo cp /etc/sysconfig/httpd /etc/sysconfig/httpd.backup
   ```

1. Use a text editor to edit the file named `/etc/sysconfig/httpd`\. Add the following line, specifying the user name and password of the cryptographic user \(CU\)\. Replace *<CU user name>* with the name of the cryptographic user\. Replace *<password>* with the CU password\.

   ```
   export n3fips_password=<CU user name>:<password>
   ```

   Save the file\. This requires Linux root permissions\.

1. Run the following command to start the Apache HTTP Server\.

   ```
   sudo service httpd start
   ```

1. Run the following command to configure your server to start Apache when the server starts\.

   ```
   sudo chkconfig httpd on
   ```

## Add the Web Server to a Security Group<a name="ssl-offload-add-security-group"></a>

To connect to your web server from a client \(such as a web browser\), create a security group that allows inbound HTTPS connections\. Specifically, it should allow inbound TCP connections on port 443\. Assign this security group to your web server\. 

**To create a security group for HTTPS and assign it to your web server**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Security Groups** in the navigation pane\.

1. Choose **Create Security Group**\.

1. For **Create Security Group**, do the following:

   1. For **Security group name**, type a name for the security group that you are creating\.

   1. \(Optional\) Type a description of the security group that you are creating\.

   1. For **VPC**, choose the VPC that contains your Amazon EC2 instance\.

   1. Choose **Add Rule**\.

   1. For **Type**, choose **HTTPS**\.

1. Choose **Create**\.

1. In the navigation pane, choose **Instances**\.

1. Select the check box next to your web server instance\. Then choose **Actions**, **Networking**, and **Change Security Groups**\.

1. Select the check box next to the security group that you created for HTTPS\. Then choose **Assign Security Groups**\.

## Verify That HTTPS Uses the Private Key in Your AWS CloudHSM Cluster<a name="ssl-offload-verify-https-connection"></a>

After you add the web server to a security group, you can verify that SSL/TLS offload with AWS CloudHSM is working\. You can do this with a web browser or with a tool such as [OpenSSL s\_client](https://www.openssl.org/docs/manmaster/man1/s_client.html)\. 

**To verify SSL/TLS offload with a web browser**

1. Use a web browser to connect to your web server using the public DNS name or IP address of the server\. Ensure that the URL in the address bar begins with https://\. For example, **https://ec2\-52\-14\-212\-67\.us\-east\-2\.compute\.amazonaws\.com/**\. 
**Note**  
You can use a DNS service such as Route 53 to route your website's domain name to your web server\. For more information, see [Routing Traffic to an Amazon EC2 Instance](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) in the *Amazon Route 53 Developer Guide* or in the documentation for your DNS service\.

1. Use your web browser to view the web server certificate\. For more information, see the following:

   + For Mozilla Firefox, see [ View a Certificate ](https://support.mozilla.org/en-US/kb/secure-website-certificate#w_view-a-certificate) on the Mozilla Support website\. 

   + For Google Chrome, see [ Understand Security Issues ](https://developers.google.com/web/tools/chrome-devtools/security) on the Google Developers website\. 

   Other web browsers might have similar features that you can use to view the web server certificate\.

1. Ensure that the SSL/TLS certificate is the one that you configured your web server to use\. For more information, see [Update the Web Server Configuration](#ssl-offload-update-web-server-configuration)\. Ensure that the private key is stored in an HSM in your AWS CloudHSM cluster\. 

**To verify SSL/TLS offload with OpenSSL s\_client**

1. Run the following OpenSSL command to connect to your web server using HTTPS\. Replace *<server name>* with the public DNS name or IP address of your web server\. 

   ```
   $ openssl s_client -connect <server name>:443
   ```

1. Ensure that the SSL/TLS certificate is the one that you configured your web server to use\. For more information, see [Update the Web Server Configuration](#ssl-offload-update-web-server-configuration)\. Ensure that the private key is stored in an HSM in your AWS CloudHSM cluster\. 

You now have a website that is secured with HTTPS\. The private key for the web server is stored in an HSM in your AWS CloudHSM cluster\. However, you have only one web server\. To set up a second web server and a load balancer for higher availability, go to [Add a Load Balancer](ssl-offload-add-load-balancing.md)\. 