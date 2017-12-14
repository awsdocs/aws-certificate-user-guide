# Removing Tags from a Certificate<a name="ct-acm-removetag"></a>

The following CloudTrail example shows the results of a call to the [RemoveTagsFromCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html) API\. 

```
{
    Records: [{
        eventVersion: "1.04",
        userIdentity: {
            type: "IAMUser",
            principalId: "AIDACKCEVSQ6C2EXAMPLE",
            arn: "arn:aws:iam::123456789012:user/Alice",
            accountId: "123456789012",
            accessKeyId: "AKIAIOSFODNN7EXAMPLE",
            userName: "Alice"
        },
        eventTime: "2016-04-06T14:10:01Z",
        eventSource: "acm.amazonaws.com",
        eventName: "RemoveTagsFromCertificate",
        awsRegion: "us-east-1",
        sourceIPAddress: "192.0.2.0",
        userAgent: "aws-cli/1.10.16",
        requestParameters: {
            certificateArn: "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012",
            tags: [{
                value: "Bob",
                key: "Admin"
            }]
        },
        responseElements: null,
        requestID: "40ded461-fc01-11e5-a747-85804766d6c9",
        eventID: "0cfa142e-ef74-4b21-9515-47197780c424",
        eventType: "AwsApiCall",
        recipientAccountId: "123456789012"
    }]
}
```