# Getting API Logs with AWS CloudTrail<a name="get-api-logs-using-cloudtrail"></a>

AWS CloudHSM is integrated with AWS CloudTrail, a service that records all AWS CloudHSM API calls in log files, and delivers those files to an Amazon Simple Storage Service \(Amazon S3\) bucket that you choose\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

CloudTrail records all calls to the AWS CloudHSM API, including those from the AWS CloudHSM console or from your code\. For the full list of AWS CloudHSM API operations, see [Actions](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_Operations.html) in the *AWS CloudHSM API Reference*\.

From the information recorded by CloudTrail, you can determine the request that was made to AWS CloudHSM, the source IP address from which the request was made, who made the request, when it was made, and so on\. CloudTrail log files contain all AWS API calls in your AWS account, not only the AWS CloudHSM API calls\.

To get started with CloudTrail, see [Getting Started with CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-getting-started.html) in the *AWS CloudTrail User Guide*\.

## Understanding AWS CloudHSM Log File Entries in CloudTrail<a name="understanding-log-file-entries"></a>

CloudTrail log files contain one or more log entries in [JSON \(JavaScript Object Notation\)](http://json.org) format\. Each log entry represents a single API call and includes information about the requested operation, the date and time of the operation, the request and response parameters, and so on\. Log entries are not an ordered stack trace of API calls, so they do not appear in any particular order\.

For more information about CloudTrail log entries, see [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html) in the *AWS CloudTrail User Guide*\.

The following example shows a CloudTrail log entry for a `CreateHsm` call to the AWS CloudHSM API\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AROAJZVM5NEGZSTCITAMM:ExampleSession",
        "arn": "arn:aws:sts::111122223333:assumed-role/AdminRole/ExampleSession",
        "accountId": "111122223333",
        "accessKeyId": "ASIAIY22AX6VRYNBGJSA",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2017-07-11T03:48:44Z"
            },
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AROAJZVM5NEGZSTCITAMM",
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