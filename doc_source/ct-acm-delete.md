# Deleting a Certificate<a name="ct-acm-delete"></a>

The following CloudTrail example shows the results of a call to the [DeleteCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html) API\. 

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
        "eventTime": "2016-03-18T00:00:26Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "DeleteCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "responseElements": null,
        "requestID": "6b0f5bb9-ec9c-11e5-a28b-51e7e3169e0f",
        "eventID": "08f18f8a-a827-4924-b864-afaf98517793",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```