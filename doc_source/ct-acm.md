# Logging AWS Certificate Manager API Calls with AWS CloudTrail<a name="ct-acm"></a>

ACM is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in ACM\. CloudTrail captures API calls for ACM as events, including calls from the ACM console and code calls to the ACM API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for ACM\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to ACM, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## ACM Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in ACM, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for ACM, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

### ACM Actions Supported in CloudTrail<a name="acm-supported-actions-in-cloudtrail"></a>

ACM supports logging the following actions as events in CloudTrail log files:
+ [AddTagsToCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_AddTagsToCertificate.html)
+ [DeleteCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html)
+ [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html)
+ [ExportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ExportCertificate.html)
+ [GetCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html)
+ [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html)
+ [ListCertificates](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificates.html)
+ [ListTagsForCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListTagsForCertificate.html)
+ [RemoveTagsFromCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html)
+ [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html)
+ [ResendValidationEmail](https://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html)

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Example ACM Log File Entries<a name="understanding-service-name-entries"></a>

 A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\.

For examples of possible ACM CloudTrail entries, see the following sections\.

### Adding Tags to a Certificate<a name="ct-acm-addtags"></a>

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

### Deleting a Certificate<a name="ct-acm-delete"></a>

The following CloudTrail example shows the results of a call to the [DeleteCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DeleteCertificate.html) API\. 

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

### Describing a Certificate<a name="ct-acm-describe"></a>

The following CloudTrail example shows the results of a call to the [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) API\. 

**Note**  
The CloudTrail log for the `DescribeCertificate` action does not display information about the ACM Certificate you specify\. You can view information about the certificate by using the console, the AWS Command Line Interface, or the [DescribeCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_DescribeCertificate.html) API\. 

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

### Exporting a Certificate<a name="ct-acm-export"></a>

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

### Import a Certificate<a name="ct-acm-import"></a>

The following example shows the CloudTrail log entry that records a call to the ACM [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html) API operation\. 

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

### Listing Certificates<a name="ct-acm-list"></a>

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

### Listing Tags for a Certificate<a name="ct-acm-listtags"></a>

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

### Removing Tags from a Certificate<a name="ct-acm-removetag"></a>

The following CloudTrail example shows the results of a call to the [RemoveTagsFromCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RemoveTagsFromCertificate.html) API\. 

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

### Requesting a Certificate<a name="ct-acm-request"></a>

The following CloudTrail example shows the results of a call to the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API\. 

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
        "eventTime": "2016-03-18T00:00:49Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "RequestCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "subjectAlternativeNames": ["example.net"],
            "domainName": "example.com",
            "domainValidationOptions": [{
                "domainName": "example.com",
                "validationDomain": "example.com"
            },
            {
                "domainName": "example.net",
                "validationDomain": "example.net"
            }],
            "idempotencyToken": "8186023d89681c3ad5"
        },
        "responseElements": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "requestID": "77dacef3-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "a4954cdb-8f38-44c7-8927-a38ad4be3ac8",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```

### Resending Validation Email<a name="ct-acm-resendmail"></a>

The following CloudTrail example shows the results of a call to the [ResendValidationEmail](https://docs.aws.amazon.com/acm/latest/APIReference/API_ResendValidationEmail.html) API\. 

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
		"eventTime": "2016-03-17T23:58:25Z",
		"eventSource": "acm.amazonaws.com",
		"eventName": "ResendValidationEmail",
		"awsRegion": "us-east-1",
		"sourceIPAddress": "192.0.2.0",
		"userAgent": "aws-cli/1.9.15",
		"requestParameters": {
			"domain": "example.com",
			"certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012",
			"validationDomain": "example.com"
		},
		"responseElements": null,
		"requestID": "23760b88-ec9c-11e5-b6f4-cb861a6f0a28",
		"eventID": "41c11b06-ca91-4c1c-8c61-af349ea8bab8",
		"eventType": "AwsApiCall",
		"recipientAccountId": "123456789012"
	}]
}
```

### Retrieving a Certificate<a name="ct-acm-get"></a>

The following CloudTrail example shows the results of a call to the [GetCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_GetCertificate.html) API\. 

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
        "eventTime": "2016-03-18T00:00:41Z",
        "eventSource": "acm.amazonaws.com",
        "eventName": "GetCertificate",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "192.0.2.0",
        "userAgent": "aws-cli/1.9.15",
        "requestParameters": {
            "certificateArn": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        },
        "responseElements": {
            "certificateChain": 
            "-----BEGIN CERTIFICATE-----
            Base64-encoded certificate chain
            -----END CERTIFICATE-----",
            "certificate": 
            "-----BEGIN CERTIFICATE-----
            Base64-encoded certificate
            -----END CERTIFICATE-----"
        },
        "requestID": "744dd891-ec9c-11e5-ac34-d1e4dfe1a11b",
        "eventID": "7aa4f909-00dd-478a-9a00-b2709bcad2bb",
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }]
}
```