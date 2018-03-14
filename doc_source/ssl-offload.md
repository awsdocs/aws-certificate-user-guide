# Improve Your Web Server's Security with SSL/TLS Offload in AWS CloudHSM<a name="ssl-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive webpages or other data over the internet\. This is commonly known as HTTPS\. The web server uses a public–private key pair and an SSL/TLS public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this to an HSM in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. Offloading reduces the computational burden on your web server and provides extra security by storing the server's private key in an HSM\. 

The [Nginx](https://nginx.org/en/) and [Apache HTTP Server](https://httpd.apache.org/) web server applications natively integrate with [OpenSSL](https://www.openssl.org/) to support HTTPS\. The [AWS CloudHSM software library for OpenSSL](openssl-library.md) provides an interface that enables the web server to use the HSMs in your cluster for cryptographic offloading and key storage\. The AWS CloudHSM software library for OpenSSL is the bridge that connects the web server to your AWS CloudHSM cluster\. 

In this tutorial, you choose whether to use the Nginx or Apache web server\. You install the web server on a Linux instance in Amazon EC2\. You can then optionally use Amazon EC2 to create a second web server and Elastic Load Balancing to create a load balancer\. Using a load balancer can increase performance by distributing the load across multiple servers\. It can also provide redundancy and higher availability if one or more servers fail\. 

For more information, see the following topics:


+ [How SSL/TLS Offload with AWS CloudHSM Works](ssl-offload-overview.md)
+ [Web Server SSL/TLS Offload Step 1: Set Up the Prerequisites](ssl-offload-prerequisites.md)
+ [Web Server SSL/TLS Offload Step 2: Import or Generate a Private Key and SSL/TLS Certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)
+ [Web Server SSL/TLS Offload Step 3: Configure the Web Server](ssl-offload-configure-web-server.md)
+ [Web Server SSL/TLS Offload Step 4: Add a Load Balancer with Elastic Load Balancing](ssl-offload-add-load-balancing.md)