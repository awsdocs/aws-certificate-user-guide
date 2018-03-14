# Creating a Load Balancer<a name="ct-related-lb"></a>

The following example shows a call to the `CreateLoadBalancer` function by an IAM user named Alice\. The name of the load balancer is `TestLinuxDefault`, and the listener is created using an ACM Certificate\. 

```
{
	"eventVersion": "1.03",
	"userIdentity": {
		"type": "IAMUser",
		"principalId": "AIDACKCEVSQ6C2EXAMPLE",
		"arn": "arn:aws:iam::111122223333:user/Alice",
		"accountId": "111122223333",
		"accessKeyId": "AKIAIOSFODNN7EXAMPLE",
		"userName": "Alice"
	},
	"eventTime": "2016-01-01T21:10:36Z",
	"eventSource": "elasticloadbalancing.amazonaws.com",
	"eventName": "CreateLoadBalancer",
	"awsRegion": "us-east-1",
	"sourceIPAddress": "192.0.2.0/24",
	"userAgent": "aws-cli/1.9.15",
	"requestParameters": {
		"availabilityZones": ["us-east-1b"],
		"loadBalancerName": "LinuxTest",
		"listeners": [{
			"sSLCertificateId": "arn:aws:acm:us-east-1:111122223333:certificate/12345678-1234-1234-1234-123456789012",
			"protocol": "HTTPS",
			"loadBalancerPort": 443,
			"instanceProtocol": "HTTP",
			"instancePort": 80
		}]
	},
	"responseElements": {
		"dNSName": "LinuxTest-1234567890.us-east-1.elb.amazonaws.com"
	},
	"requestID": "19669c3b-b0cc-11e5-85b2-57397210a2e5",
	"eventID": "5d6c00c9-a9b8-46ef-9f3b-4589f5be63f7",
	"eventType": "AwsApiCall",
	"recipientAccountId": "111122223333"
}
```