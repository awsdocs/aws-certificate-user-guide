# Import a Certificate<a name="ct-acm-import"></a>

The following example shows the CloudTrail log entry that records a call to the ACM [ImportCertificate](http://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html) API operation\. 

```
{
  "eventVersion": "1.04",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDACKCEVSQ6C2EXAMPLE",
    "arn": "arn:aws:iam::111122223333:user/Alice",
    "accountId": "111122223333",
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
    "userName": "Alice"
  },
  "eventTime": "2016-10-04T16:01:30Z",
  "eventSource": "acm.amazonaws.com",
  "eventName": "ImportCertificate",
  "awsRegion": "ap-southeast-2",
  "sourceIPAddress": "54.240.193.129",
  "userAgent": "Coral/Netty",
  "requestParameters": {
    "privateKey": {
      "hb": [
        byte,
        byte,
        byte,
        ...
      ],
      "offset": 0,
      "isReadOnly": false,
      "bigEndian": true,
      "nativeByteOrder": false,
      "mark": -1,
      "position": 0,
      "limit": 1674,
      "capacity": 1674,
      "address": 0
    },
    "certificateChain": {
      "hb": [
        byte,
        byte,
        byte,
        ...
      ],
      "offset": 0,
      "isReadOnly": false,
      "bigEndian": true,
      "nativeByteOrder": false,
      "mark": -1,
      "position": 0,
      "limit": 2105,
      "capacity": 2105,
      "address": 0
    },
    "certificate": {
      "hb": [
        byte,
        byte,
        byte,
        ...
      ],
      "offset": 0,
      "isReadOnly": false,
      "bigEndian": true,
      "nativeByteOrder": false,
      "mark": -1,
      "position": 0,
      "limit": 2503,
      "capacity": 2503,
      "address": 0
    }
  },
  "responseElements": {
    "certificateArn": "arn:aws:acm:ap-southeast-2:111122223333:certificate/6ae06649-ea82-4b58-90ee-dc05870d7e99"
  },
  "requestID": "cf1f3db7-8a4b-11e6-88c8-196af94bb7be",
  "eventID": "fb443118-bfaa-4c90-95c1-beef21e07f8e",
  "eventType": "AwsApiCall",
  "recipientAccountId": "111122223333"
}
```