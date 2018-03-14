# Getting API Logs with AWS CloudTrail<a name="hsm-metrics-ct"></a>

AWS CloudTrail records API calls in log files and delivers those files in the JSON \(JavaScript Object Notation\) format to an Amazon S3 bucket that you specify\. CloudTrail records every call users in your account make to the [AWS CloudHSM API](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_Operations.html), whether the call is made from the console, the AWS CLI, or from code\. 

**Note**  
Calls made directly to an HSM instance under the e2e encrypted channel are not visible to the AWS CloudHSM service and are not logged by CloudTrail\. 

You can use CloudTrail log files to determine what request was made to AWS CloudHSM, the source IP address from which the request was made, who made the request, when it was made, and so on\. CloudTrail log files contain all AWS API calls made in your AWS account, not only the AWS CloudHSM API calls\. For general information about CloudTrail, see [Getting Started with CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-getting-started.html) in the *AWS CloudTrail User Guide*\. For more information about CloudTrail log entries, see the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html)\. The following example shows a CloudTrail log entry for a `CreateHsm` call to the AWS CloudHSM API\. 

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "principal_ID:ExampleSession",
        "arn": "arn:aws:sts::111122223333:assumed-role/AdminRole/ExampleSession",
        "accountId": "111122223333",
        "accessKeyId": "access-key-id",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2017-07-11T03:48:44Z"
            },
            "sessionIssuer": {
                "type": "Role",
                "principalId": "principal_ID",
                "arn": "arn:aws:iam::111122223333:role/AdminRole",
                "accountId": "111122223333",
                "userName": "AdminRole"
            }
        }
    },
    "eventTime": "2017-07-11T03:50:45Z",
    "eventSource": "cloudhsm.amazonaws.com",
    "eventName": "CreateHsm",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "205.251.233.179",
    "userAgent": "aws-internal/3",
    "requestParameters": {
        "availabilityZone": "us-west-2b",
        "clusterId": "cluster-fw7mh6mayb5"
    },
    "responseElements": {
        "hsm": {
            "eniId": "eni-65338b5a",
            "clusterId": "cluster-fw7mh6mayb5",
            "state": "CREATE_IN_PROGRESS",
            "eniIp": "10.0.2.7",
            "hsmId": "hsm-6lz2hfmnzbx",
            "subnetId": "subnet-02c28c4b",
            "availabilityZone": "us-west-2b"
        }
    },
    "requestID": "1dae0370-65ec-11e7-a770-6578d63de907",
    "eventID": "b73a5617-8508-4c3d-900d-aa8ac9b31d08",
    "eventType": "AwsApiCall",
    "recipientAccountId": "111122223333"
}
```