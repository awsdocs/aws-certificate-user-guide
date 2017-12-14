# Retrieving a Certificate<a name="ct-acm-get"></a>

The following CloudTrail example shows the results of a call to the [GetCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html) API\. 

```
{
    "Records": [{
        "eventVersion": "1.04",
        "userIdentity": {
            "type": "IAMUser",
            "principalId": "AIDACKCEVSQ6C2EXAMPLE",
            "arn": "arn:aws:iam::123456789012:user/Alice",
            "accountId": "123456789012",
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
            "userName": "Alice"
        },
        "eventTime": "2016-03-18T00:00:41Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "GetCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "responseElements": {
            "certificateChain": 
            "-----BEGIN CERTIFICATE-----
            Base64-encoded certificate chain
            -----END CERTIFICATE-----",
            "certificate": 
            "-----BEGIN CERTIFICATE-----
            Base64-encoded certificate
            -----END CERTIFICATE-----"
        },
        "requestID": "744dd891-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "7aa4f909-00dd-478a-9a00-b2709bcad2bb",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```