# How SSL/TLS Offload with AWS CloudHSM Works<a name="ssl-offload-overview"></a>

To establish an HTTPS connection, your web server performs a handshake process with clients\. As part of this process, the server offloads some of the cryptographic processing to the HSMs, as shown in the following figure\. Each step of the process is explained below the figure\. 

**Note**  
The following image and process assumes that RSA is used for server verification and key exchange\. The process is slightly different when Diffie–Hellman is used instead of RSA\. 

![\[An illustration of the TLS handshake process between a client and server including cryptographic offload to an HSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/ssl-offload-handshake-process.png)

1. The client sends a hello message to the server\.

1. The server responds with a hello message and sends the server's certificate\.

1. The client performs the following actions:

   1. Verifies that the SSL/TLS server certificate is signed by a root certificate that the client trusts\. 

   1. Extracts the public key from the server certificate\.

   1. Generates a premaster secret and encrypts it with the server's public key\.

   1. Sends the encrypted premaster secret to the server\.

1. To decrypt the client's premaster secret, the server sends it to the HSM\. The HSM uses the private key in the HSM to decrypt the premaster secret and then it sends the premaster secret to the server\. Independently, the client and server each use the premaster secret and some information from the hello messages to calculate a master secret\. 

1. The handshake process ends\. For the rest of the session, all messages sent between the client and the server are encrypted with derivatives of the master secret\. 