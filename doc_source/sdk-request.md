# Requesting a Certificate<a name="sdk-request"></a>

The following example shows how to use the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) function\. 

```
package com.amazonaws.samples;

import com.amazonaws.services.certificatemanager.AWSCertificateManagerClientBuilder;
import com.amazonaws.services.certificatemanager.AWSCertificateManager;
import com.amazonaws.services.certificatemanager.model.RequestCertificateRequest;
import com.amazonaws.services.certificatemanager.model.RequestCertificateResult;

import com.amazonaws.services.certificatemanager.model.InvalidDomainValidationOptionsException;
import com.amazonaws.services.certificatemanager.model.LimitExceededException;
import com.amazonaws.AmazonClientException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.regions.Regions;

import java.util.ArrayList;

/**
 * This sample demonstrates how to use the RequestCertificate function in the AWS Certificate
 * Manager service.
 *
 * Input parameters:
 *   DomainName - FQDN of your site.
 *   DomainValidationOptions - Domain name for email validation.
 *   IdempotencyToken - Distinguishes between calls to RequestCertificate.
 *   SubjectAlternativeNames - Additional FQDNs for the subject alternative names extension.
 *
 * Output parameter:
 *   Certificate ARN - The Amazon Resource Name (ARN) of the certificate you requested.
 *
*/

public class AWSCertificateManagerExample {

   public static void main(String[] args) {

      // Retrieve your credentials from the C:\Users\name\.aws\credentials file in Windows
      // or the ~/.aws/credentials file in Linux.
      AWSCredentials credentials = null;
      try {
          credentials = new ProfileCredentialsProvider().getCredentials();
      }
      catch (Exception ex) {
          throw new AmazonClientException("Cannot load your credentials from file.", ex);
      }

      // Create a client.
      AWSCertificateManager client = AWSCertificateManagerClientBuilder.standard()
              .withRegion(Regions.US_EAST_1)
              .withCredentials(new AWSStaticCredentialsProvider(credentials))
              .build();

      // Specify a SAN.
      ArrayList<String> san = new ArrayList<String>();
      san.add("www.example.com");

      // Create a request object and set the input parameters.
      RequestCertificateRequest req = new RequestCertificateRequest();
      req.setDomainName("example.com");
      req.setIdempotencyToken("1Aq25pTy");
      req.setSubjectAlternativeNames(san);

      // Create a result object and display the certificate ARN.
      RequestCertificateResult result = null;
      try {
         result = client.requestCertificate(req);
      }
      catch(InvalidDomainValidationOptionsException ex)
      {
         throw ex;
      }
      catch(LimitExceededException ex)
      {
         throw ex;
      }

      // Display the ARN.
      System.out.println(result);

   }

}
```

The preceding sample creates output similar to the following\.

```
{CertificateArn: arn:aws:acm:region:account:certificate/12345678-1234-1234-1234-123456789012}
```