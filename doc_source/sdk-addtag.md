# Adding Tags to a Certificate<a name="sdk-addtag"></a>

The following example shows how to use the [AddTagsToCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html) function\.

```
    package com.amazonaws.samples;
     
    import java.io.IOException;
    import java.nio.ByteBuffer;
    import java.nio.charset.StandardCharsets;
    import java.nio.file.Files;
    import java.nio.file.Paths;
     
    import com.amazonaws.auth.AWSStaticCredentialsProvider;
    import com.amazonaws.auth.BasicAWSCredentials;
    import com.amazonaws.regions.Regions;
    import com.amazonaws.services.certificatemanager.AWSCertificateManager;
    import com.amazonaws.services.certificatemanager.AWSCertificateManagerClientBuilder;
    import com.amazonaws.services.certificatemanager.model.ImportCertificateRequest;
    import com.amazonaws.services.certificatemanager.model.ImportCertificateResult;
    /**
     * This sample demonstrates how to use the ImportCertificate function in the AWS Certificate Manager 
     * service.
     *
     * Input parameters:
     *   Accesskey - AWS access key
     *   SecretKey - AWS secret key
     *   CertificateArn - Use to reimport a certificate (not included in this example).
     *   region - AWS region
     *   Certificate - PEM file that contains the certificate to import. Ex: /data/certs/servercert.pem
     *   CertificateChain - The certificate chain, not including the end-entity certificate.
     *   PrivateKey - The private key that matches the public key in the certificate.
     *
     * Output parameter:
     *   CertificcateArn - The ARN of the imported certificate.
     *
     */
    public class AWSCertificateManagerSample {
     
    	public static void main(String[] args) throws IOException {
    		String accessKey = "";
    		String secretKey = "";
    		String certificateArn = null;
    		Regions region = Regions.DEFAULT_REGION;
    		String serverCertFilePath = "";
    		String privateKeyFilePath = "";
    		String caCertFilePath = "";
     
    		ImportCertificateRequest req = new ImportCertificateRequest()
    				.withCertificate(getCertContent(serverCertFilePath))
    				.withPrivateKey(getCertContent(privateKeyFilePath))
    				.withCertificateChain(getCertContent(caCertFilePath)).withCertificateArn(certificateArn);
     
    		AWSCertificateManager client = AWSCertificateManagerClientBuilder.standard().withRegion(region)
    				.withCredentials(new AWSStaticCredentialsProvider(new BasicAWSCredentials(accessKey, secretKey)))
    				.build();
    		ImportCertificateResult result = client.importCertificate(req);
     
    		System.out.println(result.getCertificateArn());
     
    		List<Tag> expectedTags = ImmutableList.of(Tag.builder().withKey("key").withValue("value").build());
     
    		AddTagsToCertificateRequest addTagsToCertificateRequest = AddTagsToCertificateRequest.builder()
        		    .withCertificateArn(result.getCertificateArn())
        		    .withTags(tags)
        		    .build();
     
    		client.addTagsToCertificate(addTagsToCertificateRequest);
    	}
     
    	private static ByteBuffer getCertContent(String filePath) throws IOException {
    		String fileContent = new String(Files.readAllBytes(Paths.get(filePath)));
    		return StandardCharsets.UTF_8.encode(fileContent);
    	}
    }
```