# Responding to an event with a Lambda function<a name="event-lambda-response"></a>

This procedure demonstrates how to use AWS Lambda to listen on CloudWatch Events, create notifications with Amazon Simple Notification Service \(SNS\), and publish findings to AWS Security Hub, providing visibility to administrators and security teams\. <a name="lambda-setup"></a>

**To set up a Lambda function and IAM role**

1. First configure an AWS Identity and Access Management \(IAM\) role and define the permissions needed by the Lambda function\. This security best practice gives you flexibility in designating who has authorization to call the function, and in limiting the permissions granted to that person\. It is not recommended to run most AWS operations directly under a user account and especially not under an administrator account\.

   Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Use the JSON policy editor to create the policy defined in the template below\. Provide your own Region and AWS account details\. For more information, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/lambda/latest/dg/access_policies_create-console.html#access_policies_create-json-editor)\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"LambdaCertificateExpiryPolicy1",
            "Effect":"Allow",
            "Action":"logs:CreateLogGroup",
            "Resource":"arn:aws:logs:<region>:<AWS-ACCT-NUMBER>:*"
         },
         {
            "Sid":"LambdaCertificateExpiryPolicy2",
            "Effect":"Allow",
            "Action":[
               "logs:CreateLogStream",
               "logs:PutLogEvents"
            ],
            "Resource":[
               "arn:aws:logs:<region>:<AWS-ACCT-NUMBER>:log-group:/aws/lambda/handle-expiring-certificates:*"
            ]
         },
         {
            "Sid":"LambdaCertificateExpiryPolicy3",
            "Effect":"Allow",
            "Action":[
               "acm:DescribeCertificate",
               "acm:GetCertificate",
               "acm:ListCertificates",
               "acm:ListTagsForCertificate"
            ],
            "Resource":"*"
         },
         {
            "Sid":"LambdaCertificateExpiryPolicy4",
            "Effect":"Allow",
            "Action":"SNS:Publish",
            "Resource":"*"
         },
         {
            "Sid":"LambdaCertificateExpiryPolicy5",
            "Effect":"Allow",
            "Action":[
               "SecurityHub:BatchImportFindings",
               "SecurityHub:BatchUpdateFindings",
               "SecurityHub:DescribeHub"
            ],
            "Resource":"*"
         },
         {
            "Sid":"LambdaCertificateExpiryPolicy6",
            "Effect":"Allow",
            "Action":"cloudwatch:ListMetrics",
            "Resource":"*"
         }
      ]
   }
   ```

1. Create an IAM role and attach the new policy to it\. For information about creating an IAM role and attaching a policy, see [Creating a role for an AWS service \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console)\. 

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Create the Lambda function\. For more information, see [Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\. Complete the following steps:

   1. On the **Create function** page, choose the **Author from scratch** option to create the function\.

   1. Specify a name such as "handle\-expiring\-certificates" in the **Function name** field\.

   1. Choose Python 3\.8 from the **Runtime** list\.

   1. Expand **Change default execution role** and choose **Use an existing role**\.

   1. Choose the role you previously created from the **Existing role** list\.

   1. Choose **Create function**\.

   1. Under **Function code**, insert the following code:

      ```
      # Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
      # SPDX-License-Identifier: MIT-0
      #
      # Permission is hereby granted, free of charge, to any person obtaining a copy of this
      # software and associated documentation files (the "Software"), to deal in the Software
      # without restriction, including without limitation the rights to use, copy, modify,
      # merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
      # permit persons to whom the Software is furnished to do so.
      #
      # THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
      # INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
      # PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
      # HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
      # OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
      # SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
      
      import json
      import boto3
      import os
      from datetime import datetime, timedelta, timezone
      # -------------------------------------------
      # setup global data
      # -------------------------------------------
      utc = timezone.utc
      # make today timezone aware
      today = datetime.now().replace(tzinfo=utc)
      # set up time window for alert - default to 45 if its missing
      if os.environ.get('EXPIRY_DAYS') is None:
          expiry_days = 45
      else:
          expiry_days = int(os.environ['EXPIRY_DAYS'])
      expiry_window = today + timedelta(days = expiry_days)
      def lambda_handler(event, context):
          # if this is coming from the ACM event, its for a single certificate
          if (event['detail-type'] == "ACM Certificate Approaching Expiration"):
              response = handle_single_cert(event, context.invoked_function_arn)
          return {
              'statusCode': 200,
              'body': response 
          }
      def handle_single_cert(event, context_arn):
          cert_client = boto3.client('acm')
          cert_details = cert_client.describe_certificate(CertificateArn=event['resources'][0])
          result = 'The following certificate is expiring within ' + str(expiry_days) + ' days: ' + cert_details['Certificate']['DomainName']
          # check the expiry window before logging to Security Hub and sending an SNS
          if cert_details['Certificate']['NotAfter'] < expiry_window:
              # This call is the text going into the SNS notification
              result = result + ' (' + cert_details['Certificate']['CertificateArn'] + ') '
              # this call is publishing to SH
              result = result + ' - ' + log_finding_to_sh(event, cert_details, context_arn)
              # if there's an SNS topic, publish a notification to it
              if os.environ.get('SNS_TOPIC_ARN') is None:
                  response = result
              else:
                  sns_client = boto3.client('sns')
                  response = sns_client.publish(TopicArn=os.environ['SNS_TOPIC_ARN'], Message=result, Subject='Certificate Expiration Notification')
          return result
      def log_finding_to_sh(event, cert_details, context_arn):
          # setup for security hub
          sh_region = get_sh_region(event['region'])
          sh_hub_arn = "arn:aws:securityhub:{0}:{1}:hub/default".format(sh_region, event['account'])
          sh_product_arn = "arn:aws:securityhub:{0}:{1}:product/{1}/default".format(sh_region, event['account'])
          # check if security hub is enabled, and if the hub arn exists
          sh_client = boto3.client('securityhub', region_name = sh_region)
          try:
              sh_enabled = sh_client.describe_hub(HubArn = sh_hub_arn)
          # the previous command throws an error indicating the hub doesn't exist or lambda doesn't have rights to it so we'll stop attempting to use it
          except Exception as error:
              sh_enabled = None
              print ('Default Security Hub product doesn\'t exist')
              response = 'Security Hub disabled'
          # This is used to generate the URL to the cert in the Security Hub Findings to link directly to it
          cert_id = right(cert_details['Certificate']['CertificateArn'], 36)
          if sh_enabled:
              # set up a new findings list
              new_findings = []
                  # add expiring certificate to the new findings list
              new_findings.append({
                  "SchemaVersion": "2018-10-08",
                  "Id": cert_id,
                  "ProductArn": sh_product_arn,
                  "GeneratorId": context_arn,
                  "AwsAccountId": event['account'],
                  "Types": [
                      "Software and Configuration Checks/AWS Config Analysis"
                  ],
                  "CreatedAt": event['time'],
                  "UpdatedAt": event['time'],
                  "Severity": {
                      "Original": '89.0',
                      "Label": 'HIGH'
                  },
                  "Title": 'Certificate expiration',
                  "Description": 'cert expiry',
                  'Remediation': {
                      'Recommendation': {
                          'Text': 'A new certificate for ' + cert_details['Certificate']['DomainName'] + ' should be imported to replace the existing imported certificate before expiration',
                          'Url': "https://console.aws.amazon.com/acm/home?region=" + event['region'] + "#/?id=" + cert_id
                      }
                  },
                  'Resources': [
                      {
                          'Id': event['id'],
                          'Type': 'ACM Certificate',
                          'Partition': 'aws',
                          'Region': event['region']
                      }
                  ],
                  'Compliance': {'Status': 'WARNING'}
              })
              # push any new findings to security hub
              if new_findings:
                  try:
                      response = sh_client.batch_import_findings(Findings=new_findings)
                      if response['FailedCount'] > 0:
                          print("Failed to import {} findings".format(response['FailedCount']))
                  except Exception as error:
                      print("Error: ", error)
                      raise
          return json.dumps(response)
      # function to setup the sh region    
      def get_sh_region(event_region):
          # security hub findings may need to go to a different region so set that here
          if os.environ.get('SECURITY_HUB_REGION') is None:
              sh_region_local = event_region
          else:
              sh_region_local = os.environ['SECURITY_HUB_REGION']
          return sh_region_local
      # quick function to trim off right side of a string
      def right(value, count):
          # To get right part of string, use negative first index in slice.
          return value[-count:]
      ```

   1. Under **Environment variables**, choose **Edit** and optionally add the following variables\.
      + \(Optional\) EXPIRY\_DAYS

        Specifies how much lead time, in days, before the certificate expiration notice is sent\. The function defaults to 45 days, but you can specify custom values\.
      + \(Optional\) SNS\_TOPIC\_ARN

        Specifies an ARN for an Amazon SNS\. Provide the full ARN in the format of arn:aws:sns:*<region>*:*<account\-number>*:*<topic\-name>*\.
      + \(Optional\) SECURITY\_HUB\_REGION

        Specifies an AWS Security Hub in a different Region\. If this is not specified, the Region of the running Lambda function is used\. If the function is run in multiple Regions, it may be desirable to have all certificate messages go to Security Hub in a single Region\. 

   1. Under **Basic settings**, set **Timeout** to 30 seconds\.

   1. At the top of the page, choose **Deploy**\.

Complete the tasks in the following procedure to begin using this solution\.

**To automate an email notice of expiration**

In this example, we provide a single email for each expiring certificate at the moment the event is raised through CloudWatch Events\. By default, ACM raises an event each day for a certificate that is 45 days or less from expiration\. \(This period can be customized using the [https://docs.aws.amazon.com/acm/latest/APIReference/](https://docs.aws.amazon.com/acm/latest/APIReference/) operation of the ACM API\.\) Each of these events triggers the following cascade of automated actions:

```
ACM raises CloudWatch event → 
>>>>>>> events

          Event matches CloudWatch rule → 

                    Rule calls Lambda function → 

                              Function sends SNS email and logs a Finding in Security Hub
```

1. Create the Lambda function and configure permissions\. \(Already completed – see [To set up a Lambda function and IAM role](#lambda-setup)\)\.

1. Create a *standard* SNS topic for the Lambda function to use to send out notifications\. For more information, see [Creating an Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html)\.

1. Subscribe any interested parties to the new SNS topic\. For more information, see [Subscribing to an Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-subscribe-endpoint-to-topic.html)\.

1. Create a CloudWatch Events rule to trigger the Lambda function\. For more information, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\.

   In the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/), navigate to **Events** >**Rules** page and choose **Create rule**\. Specify **Service Name**, **Event Type**, and **Lambda function**\. In the **Event Pattern preview** editor, paste the following code:

   ```
   {
     "source": [
       "aws.acm"
     ],
     "detail-type": [
       "ACM Certificate Approaching Expiration"
     ]
   }
   ```

   An event such as Lambda receives is displayed under **Show sample event\(s\)**:

   ```
   {
     "version": "0",
     "id": "9c95e8e4-96a4-ef3f-b739-b6aa5b193afb",
     "detail-type": "ACM Certificate Approaching Expiration",
     "source": "aws.acm",
     "account": "123456789012",
     "time": "2020-09-30T06:51:08Z",
     "region": "us-east-1",
     "resources": [
       "arn:aws:acm:us-east-1:123456789012:certificate/61f50cd4-45b9-4259-b049-d0a53682fa4b"
     ],
     "detail": {
       "DaysToExpiry": 31,
       "CommonName": "My Awesome Service"
     }
   }
   ```

**To clean up**

Once you no longer need the example configuration, or any configuration, it is a best practice to remove all traces of it to avoid security problems and unexpected future charges:
+ IAMpolicy and role
+ Lambda function
+ CloudWatch Events rule
+ CloudWatch Logs associated with Lambda
+ SNS Topic