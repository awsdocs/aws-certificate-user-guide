# Requesting a Certificate<a name="ct-acm-request"></a>

The following CloudTrail example shows the results of a call to the [RequestCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API\. 

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
        "eventTime": "2016-03-18T00:00:49Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "RequestCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "subjectAlternativeNames": ["example.net"],
            "domainName": "example.com",
            "domainValidationOptions": [{
                "domainName": "example.com",
                "validationDomain": "example.com"
            },
            {
                "domainName": "example.net",
                "validationDomain": "example.net"
            }],
            "idempotencyToken": "8186023d89681c3ad5"
        },
        "responseElements": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "requestID": "77dacef3-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "a4954cdb-8f38-44c7-8927-a38ad4be3ac8",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```