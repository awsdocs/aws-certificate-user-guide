# Encrypting a Private Key<a name="ct-related-encrypt"></a>

The following example shows an `Encrypt` call that encrypts the private key associated with an ACM Certificate\. Encryption is performed within AWS\. 

```
{
    "Records": [
    {
        "eventVersion": "1.03",
        "userIdentity": {
            "type": "IAMUser",
            "principalId": "AIDACKCEVSQ6C2EXAMPLE",
            "arn": "arn:aws:iam::111122223333:user/acm",
            "accountId": "111122223333",
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
            "userName": "acm"
        },
        "eventTime": "2016-01-05T18:36:29Z",
        "eventSource": "kms.amazonaws.com",
        "eventName": "Encrypt",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "AWS Internal",
        "userAgent": "aws-internal",
        "requestParameters": {
            "keyId": "arn:aws:kms:us-east-1:123456789012:alias/aws/acm",
            "encryptionContext": {
                "aws:acm:arn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
            }
        },
        "responseElements": null,
        "requestID": "3c417351-b3db-11e5-9a24-7d9457362fcc",
        "eventID": "1794fe70-796a-45f5-811b-6584948f24ac",
        "readOnly": true,
        "resources": [{
            "ARN": "arn:aws:kms:us-east-1:123456789012:key/87654321-4321-4321-4321-210987654321",
            "accountId": "123456789012"
        }],
        "eventType": "AwsServiceEvent",
        "recipientAccountId": "123456789012"
    }]
}
```