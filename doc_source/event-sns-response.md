# Responding to an event with Amazon SNS<a name="event-sns-response"></a>

This section shows how to configure Amazon SNS to send a text notification whenever ACM generates a health event\.

Complete the following procedure to configure a response\.

**To create a CloudWatch Events rule and trigger an action**

1. Create a CloudWatch Events rule\. For more information, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\.

   1. In the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/), navigate to the **Events** > **Rules** page and choose **Create rule**\. 

   1. On the **Create rule** page, select **Event Pattern**\.

   1. For **Service Name**, choose **Health** from the menu\.

   1. For **Event Type**, choose **Specific Health events**\.

   1. Select **Specific service\(s\)** and choose **ACM** from the menu\.

   1. Select **Specific event type category\(s\)** and choose **accountNotification**\.

   1. Choose **Any event type code**\.

   1. Choose **Any resource**\.

   1. In the **Event pattern preview** editor, paste the JSON pattern emitted by the event\. This example uses the pattern from the [AWS health events](supported-events.md#health-event) section\.

   ```
   {
      "source":[
         "aws.health"
      ],
      "detail-type":[
         "AWS Health Event"
      ],
      "detail":{
         "service":[
            "ACM"
         ],
         "eventTypeCategory":[
            "scheduledChange"
         ],
         "eventTypeCode":[
            "AWS_ACM_RENEWAL_STATE_CHANGE"
         ]
      }
   }
   ```

1. Configure an action\.

   In the **Targets** section, you can choose from among many services that can immediately consume your event, such as Amazon Simple Notification Service \(SNS\), or you can choose **Lambda function** to pass the event to customized executable code\. For an example of an AWS Lambda implementation, see [Responding to an event with a Lambda function](event-lambda-response.md)\.