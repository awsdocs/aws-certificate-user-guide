# Listing Tags for a Certificate<a name="ct-acm-listtags"></a>

The following CloudTrail example shows the results of a call to the [ListTagsForCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html) API\. 

**Note**  
The CloudTrail log for the `ListTagsForCertificate` action does not display your tags\. You can view the tag list by using the console, the AWS Command Line Interface, or the [ListTagsForCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html) API\. 

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
        eventTime: "2016-04-06T13:30:11Z",
        eventSource: "acm.amazonaws.com",
        eventName: "ListTagsForCertificate",
        awsRegion: "us-east-1",
        sourceIPAddress: "192.0.2.0",
        userAgent: "aws-cli/1.10.16",
        requestParameters: {
            certificateArn: "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        responseElements: null,
        requestID: "b010767f-fbfb-11e5-b596-79e9a97a2544",
        eventID: "32181be6-a4a0-48d3-8014-c0d972b5163b",
        eventType: "AwsApiCall",
        recipientAccountId: "123456789012"
    }]
}
```