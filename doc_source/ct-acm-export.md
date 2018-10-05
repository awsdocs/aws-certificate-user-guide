# Exporting a Certificate<a name="ct-acm-export"></a>

The following CloudTrail example shows the results of a call to the [ExportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ExportCertificate.html) API\. 

```
{
  "Records": [{
  "version": "0",
  "id": "12345678-1234-1234-1234-123456789012"
  "detail-type": "AWS API Call via CloudTrail",
  "source": "aws.acm",
  "account": "123456789012",
  "time": "2018-05-24T15:28:11Z",
  "region": "us-east-1",
  "resources": [],
  "detail": {
    "eventVersion": "1.04",
    "userIdentity": {
      "type": "Root",
      "principalId": "123456789012",
      "arn": "arn:aws:iam::123456789012:user/Alice",
      "accountId": "123456789012",
      "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "userName": "Alice"
    },
    "eventTime": "2018-05-24T15:28:11Z",
    "eventSource": "acm.amazonaws.com",
    "eventName": "ExportCertificate",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "aws-cli/1.15.4 Python/2.7.9 Windows/8 botocore/1.10.4",
    "requestParameters": {
      "passphrase": {
        "hb": [42,
        42,
        42,
        42,
        42,
        42,
        42,
        42,
        42,
        42],
        "offset": 0,
        "isReadOnly": false,
        "bigEndian": true,
        "nativeByteOrder": false,
        "mark": -1,
        "position": 0,
        "limit": 10,
        "capacity": 10,
        "address": 0
      },
      "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
    },
    "responseElements": {
      "certificateChain": "-----BEGIN CERTIFICATE----- base64 certificate -----END CERTIFICATE-----\n"
      -----BEGIN CERTIFICATE----- base64 certificate -----END CERTIFICATE-----\n",
      "privateKey": "**********",
      "certificate": "-----BEGIN CERTIFICATE----- base64 certificate -----END CERTIFICATE-----\n"
    },
    "requestID": "11802113-5f67-11e8-bc6b-d93a70b3bedf",
    "eventID": "5b66558e-27c5-43b0-9b3a-10f28c527453",
    "eventType": "AwsApiCall"
  }
}]
```