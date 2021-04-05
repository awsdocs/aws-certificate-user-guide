# Supported CloudWatch metrics<a name="cloudwatch-metrics"></a>

Amazon CloudWatch is a monitoring service for AWS resources\. You can use CloudWatch to collect and track metrics, set alarms, and automatically react to changes in your AWS resources\. AWS Certificate Manager supports the following CloudWatch metrics\. 

The `AWS/CertificateManager` namespace includes the following metric\. 


****  

| Metric | Description | Unit | Dimensions | 
| --- | --- | --- | --- | 
| DaysToExpiry | Number of days until a certificate expires\. ACM stops publishing this metric after a certificate expires\. | Integer | CertificateArn[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/acm/latest/userguide/cloudwatch-metrics.html) | 

For more information about CloudWatch metrics, see the following topics:


+ [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
+ [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)