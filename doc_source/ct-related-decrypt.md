# Decrypting a Private Key<a name="ct-related-decrypt"></a>

The following example shows a `Decrypt` call that decrypts the private key associated with an ACM Certificate\. Decryption is performed within AWS, and the decrypted key never leaves AWS\. 

```
    {
        "eventVersion": "1.03",
        "userIdentity": {
            "type": "AssumedRole",
            "principalId": "AIDACKCEVSQ6C2EXAMPLE:1aba0dc8b3a728d6998c234a99178eff",
            "arn": "arn:aws:sts::111122223333:assumed-role/DecryptACMCertificate/1aba0dc8b3a728d6998c234a99178eff",
            "accountId": "111122223333",
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
            "sessionContext": {
                "attributes": {
                    "mfaAuthenticated": "false",
                    "creationDate": "2016-01-01T21:13:28Z"
                },
                "sessionIssuer": {
                    "type": "Role",
                    "principalId": "APKAEIBAERJR2EXAMPLE",
                    "arn": "arn:aws:iam::111122223333:role/DecryptACMCertificate",
                    "accountId": "111122223333",
                    "userName": "DecryptACMCertificate"
                }
            }
        },
        "eventTime": "2016-01-01T21:13:28Z",
        "eventSource": "kms.amazonaws.com",
        "eventName": "Decrypt",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "AWS Internal",
        "userAgent": "aws-internal/3",
        "requestParameters": {
            "encryptionContext": {
                "aws:elasticloadbalancing:arn": "arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/LinuxTest",
                "aws:acm:arn": "arn:aws:acm:us-east-1:123456789012:certificate/87654321-4321-4321-4321-210987654321"
            }
        },
        "responseElements": null,
        "requestID": "809a70ff-b0cc-11e5-8f42-c7fdf1cb6e6a",
        "eventID": "7f89f7a7-baff-4802-8a88-851488607fb9",
        "readOnly": true,
        "resources": [{
            "ARN": "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012",
            "accountId": "123456789012"
        }],
        "eventType": "AwsServiceEvent",
        "recipientAccountId": "123456789012"
    }
```