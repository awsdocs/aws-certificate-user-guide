# Resending Validation Email<a name="ct-acm-resendmail"></a>

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