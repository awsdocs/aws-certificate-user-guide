# Using CloudWatch Events<a name="cloudwatch-events"></a>

You can use [Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) to automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to CloudWatch Events in near\-real time\. You can write simple rules to indicate which events are of interest to you and the automated actions to take when an event matches a rule\. For more information, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\. 

CloudWatch Events are turned into actions using Amazon EventBridge\. With EventBridge, you can use events to trigger targets including AWS Lambda functions, AWS Batch jobs, Amazon SNS topics, and many others\. For more information, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)

**Topics**
+ [Amazon EventBridge support for ACM](supported-events.md)
+ [Triggering actions with CloudWatch Events in ACM](example-actions.md)