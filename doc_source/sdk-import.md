# Importing a Certificate<a name="sdk-import"></a>

The following example shows how to use the [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html) function\. 

```
package com.amazonaws.samples;

import com.amazonaws.services.certificatemanager.AWSCertificateManagerClientBuilder;
import com.amazonaws.services.certificatemanager.AWSCertificateManager;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.regions.Regions;

import com.amazonaws.services.certificatemanager.model.ImportCertificateRequest;
import com.amazonaws.services.certificatemanager.model.ImportCertificateResult;
import com.amazonaws.services.certificatemanager.model.LimitExceededException;
import com.amazonaws.services.certificatemanager.model.ResourceNotFoundException;
import com.amazonaws.AmazonClientException;
import java.io.FileNotFoundException;
import java.io.IOException;

import java.io.RandomAccessFile;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * This sample demonstrates how to use the ImportCertificate function in the AWS Certificate Manager 
 * service.
 *
 * Input parameters:
 *   Certificate - PEM file that contains the certificate to import.
 *   CertificateArn - Use to reimport a certificate (not included in this example).
 *   CertificateChain - The certificate chain, not including the end-entity certificate.
 *   PrivateKey - The private key that matches the public key in the certificate.
 *
 * Output parameter:
 *   CertificcateArn - The ARN of the imported certificate.
 *
 */
public class AWSCertificateManagerSample {

   public static void main(String[] args) throws Exception {

      // Retrieve your credentials from the C:\Users\name\.aws\credentials file in Windows
      // or the ~/.aws/credentials file in Linux.
      AWSCredentials credentials = null;
      try {
          credentials = new ProfileCredentialsProvider().getCredentials();
      }
      catch (Exception ex) {
          throw new AmazonClientException(
              "Cannot load the credentials from file.", ex);
      }

      // Create a client.
      AWSCertificateManager client = AWSCertificateManagerClientBuilder.standard()
              .withRegion(Regions.US_EAST_1)
              .withCredentials(new AWSStaticCredentialsProvider(credentials))
              .build();

      // Initialize the file descriptors.
      RandomAccessFile file_certificate = null;
      RandomAccessFile file_chain = null;
      RandomAccessFile file_key = null;

      // Initialize the buffers.
      ByteBuffer buf_certificate = null;
      ByteBuffer buf_chain = null;
      ByteBuffer buf_key = null;

      // Create the file streams for reading.
      try {
         file_certificate = new RandomAccessFile("C:\\Temp\\certificate.pem", "r");
         file_chain = new RandomAccessFile("C:\\Temp\\chain.pem", "r");
         file_key = new RandomAccessFile("C:\\Temp\\private_key.pem", "r");
      }
      catch (IllegalArgumentException ex) {
         throw ex;
      }
      catch (SecurityException ex) {
         throw ex;
      }
      catch (FileNotFoundException ex) {
         throw ex;
      }

      // Create channels for mapping the files.
      FileChannel channel_certificate = file_certificate.getChannel();
      FileChannel channel_chain = file_chain.getChannel();
      FileChannel channel_key = file_key.getChannel();

      // Map the files to buffers.
      try {
         buf_certificate = channel_certificate.map(FileChannel.MapMode.READ_ONLY, 0, channel_certificate.size());
         buf_chain = channel_chain.map(FileChannel.MapMode.READ_ONLY, 0, channel_chain.size());
         buf_key = channel_key.map(FileChannel.MapMode.READ_ONLY, 0, channel_key.size());

         // The files have been mapped, so clean up.
         channel_certificate.close();
         channel_chain.close();
         channel_key.close();
         file_certificate.close();
         file_chain.close();
         file_key.close();
      }
      catch (IOException ex)
      {
         throw ex;
      }

      // Create a request object and set the parameters.
      ImportCertificateRequest req = new ImportCertificateRequest();
      req.setCertificate(buf_certificate);
      req.setCertificateChain(buf_chain);
      req.setPrivateKey(buf_key);

      // Import the certificate.
      ImportCertificateResult result = null;
      try {
         result = client.importCertificate(req);
      }
      catch(LimitExceededException ex)
      {
         throw ex;
      }
      catch (ResourceNotFoundException ex)
      {
         throw ex;
      }

      // Clear the buffers.
      buf_certificate.clear();
      buf_chain.clear();
      buf_key.clear();

      // Retrieve and display the certificate ARN.
      String arn = result.getCertificateArn();
      System.out.println(arn);
    }
}
```