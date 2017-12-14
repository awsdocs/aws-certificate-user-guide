# Describing a Certificate<a name="ct-acm-describe"></a>

The following CloudTrail example shows the results of a call to the [DescribeCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) API\. 

**Note**  
The CloudTrail log for the `DescribeCertificate` action does not display information about the ACM Certificate you specify\. You can view information about the certificate by using the console, the AWS Command Line Interface, or the [DescribeCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) API\. 

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
        "eventTime": "2016-03-18T00:00:42Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "DescribeCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "responseElements": null,
        "requestID": "74b91d83-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "7779b6da-75c2-4994-b8c1-af3ad47b518a",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```