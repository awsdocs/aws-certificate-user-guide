# Registering an Amazon EC2 Instance with a Load Balancer<a name="ct-related-ec2"></a>

When you provision your website or application on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, the load balancer must be made aware of that instance\. This can be accomplished through the Elastic Load Balancing console or the AWS Command Line Interface\. The following example shows a call to `RegisterInstancesWithLoadBalancer` for a load balancer named LinuxTest on AWS account 123456789012\. 

```
{
    "eventVersion": "1.03",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "arn": "arn:aws:iam::123456789012:user/ALice",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "Alice",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2016-01-01T19:35:52Z"
            }
        },
        "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2016-01-01T21:11:45Z",
    "eventSource": "elasticloadbalancing.amazonaws.com",
    "eventName": "RegisterInstancesWithLoadBalancer",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0/24",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
        "loadBalancerName": "LinuxTest",
        "instances": [{
            "instanceId": "i-c67f4e78"
        }]
    },
    "responseElements": {
        "instances": [{
            "instanceId": "i-c67f4e78"
        }]
    },
    "requestID": "438b07dc-b0cc-11e5-8afb-cda7ba020551",
    "eventID": "9f284ca6-cbe5-42a1-8251-4f0e6b5739d6",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```