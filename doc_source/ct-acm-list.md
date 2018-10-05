# Listing Certificates<a name="ct-acm-list"></a>

The following CloudTrail example shows the results of a call to the [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html) API\. 

**Note**  
The CloudTrail log for the `ListCertificates` action does not display your ACM certificates\. You can view the certificate list by using the console, the AWS Command Line Interface, or the [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html) API\. 

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
        "eventTime": "2016-03-18T00:00:43Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "ListCertificates",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "maxItems": 1000,
            "certificateStatuses": ["ISSUED"]
        },
        "responseElements": null,
        "requestID": "74c99844-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "cdfe1051-88aa-4aa3-8c33-a325270bff21",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```