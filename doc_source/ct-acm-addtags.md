# Adding Tags to a Certificate<a name="ct-acm-addtags"></a>

The following CloudTrail example shows the results of a call to the [AddTagsToCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html) API\. 

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
        eventTime: "2016-04-06T13:53:53Z",
        eventSource: "acm.amazonaws.com",
        eventName: "AddTagsToCertificate",
        awsRegion: "us-east-1",
        sourceIPAddress: "192.0.2.0",
        userAgent: "aws-cli/1.10.16",
        requestParameters: {
            tags: [{
                value: "Alice",
                key: "Admin"
            }],
            certificateArn: "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        responseElements: null,
        requestID: "ffd7dd1b-fbfe-11e5-ba7b-5f4e988901f9",
        eventID: "4e7b10bb-7010-4e60-8376-0cac3bc860a5",
        eventType: "AwsApiCall",
        recipientAccountId: "123456789012"
    }]
}
```