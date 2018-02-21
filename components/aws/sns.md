# ![SNS Logo](images/sns/sns_icon.svg) AWS SNS (Simple Notification Service)


Amazon Simple Notification Service (SNS) is a flexible, fully managed pub-sub messaging and mobile notifications service for coordinating the delivery of messages to subscribing endpoints and clients. With SNS you can fan-out messages to a large number of subscribers, including distributed systems and services, and mobile devices.

The Sigma IDE currently supports the following 3 types of SNS resources.
- SNS Topics (as triggers as well as for operations)
- Direct SMS via SNS (for operations)
- SNS Platform Applications  (for operations)

## SNS as a Trigger

A SNS topic can be used as a trigger for a lambda function within Sigma. For that, a SNS resource should be dragged from the resources panel and dropped on top of the `event` parameter of the lambda handler. Then the SNS topic configuration panel can be used to [set an SNS topic](#set-topic)  as the trigger.

When a SNS topic is configured as a trigger to a Lambda function, that function is invoked each time a message is published to the topic. The structure of trigger event received by Lambda function is of the following format.

````
{
   Records:[
      {
         EventSource:'aws:sns',
         EventVersion:'1.0',
         EventSubscriptionArn:'arn:aws:sns:us-east-1:111111111111:my_topic:9e72cbc4-8557-4c3e-97d5-fd805f7b9e1a',
         Sns:{
            Type:'Notification',
            MessageId:'81559821-47a5-5241-abd2-cb26179d5550',
            TopicArn:'arn:aws:sns:us-east-1:111111111111:my_topic',
            Subject:'Message Subject',
            Message:'Message Content',
            Timestamp:'2018-02-21T10:46:16.596Z',
            SignatureVersion:'1',
            Signature:'LUxjiNldCBpq1WJYHM8l+3rSRiXHIdx4HgI4+IYkkzMh46ev5j0DKTw4AvJPd0XMbySVtSVA4hWYhe6sPg==',
            SigningCertUrl:'https://sns.us-east-1.amazonaws.com/SimpleNotificationService-433026a4050d206028891664da859041.pem',
            UnsubscribeUrl:'https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:111111111111:my_topic:9e72cbc4-8557-4c3e-97d5-fd805f7b9e1a',
            MessageAttributes:{
               custom_attr_1:{
                  Type:'Number',
                  Value:'1234'
               },
               'AWS.SNS.MOBILE.MPNS.Type':{
                  Type:'String',
                  Value:'token'
               },
               'AWS.SNS.MOBILE.MPNS.NotificationClass':{
                  Type:'String',
                  Value:'realtime'
               },
               'AWS.SNS.MOBILE.WNS.Type':{
                  Type:'String',
                  Value:'wns/badge'
               }
            }
         }
      }
   ]
}
````


### <a name="set-topic">Setting the SNS topic

In SNS topic configuration panel, it is possible either to select an existing SNS topic or to define a new SNS topic.

#### Selecting an existing topic

<p align="center">
  <img width="400" src="./images/sns/existing_topic.png">
</p>

To select an existing topic, first go to the **Existing Topic** tab of the configuration panel. Then the **Topic Name** drop-down will be populated with all the already defined SNS topics in your AWS account for the current project region. You can simply select the required topic from that list.

#### Defining a new topic

<p align="center">
  <img width="400" src="./images/sns/new_topic.png">
</p>

To define a new topic, first go to the **New Topic** tab of the configuration panel. Then a **Topic Name** should be provided, and this topic name must be non-empty and should contain only alphanumeric characters, hyphens (-), or underscores (\_). Then a **Display Name** also should be provided and it should less than 10 characters in length.

### Adding subscriptions to a topic

Sigma also provides the facility to add new subscriptions to an existing or a newly defined topic. For that, first click on the **Subscriptions** button below the topic configuration fields, which will open the subscriptions panel of the topic.

<p align="center">
  <img width="400" src="./images/sns/subscriptions.png">
</p>

If you are using an existing topic which already has any subscriptions, they will be shown under **Existing Subscriptions**, and they cannot be changed. The new subscriptions you have defined through Sigma will be shown under **New Subscriptions** and any one of them can be removed by clicking on the remove button in-front.

To add a new subscription, first a subscription protocol should be selected from the drop-down on the left and then a suitable subscription endpoint should be specified in the right text box. Currently the following types of subscription protocols are supported.
- HTTP
- HTTPS
- Email
- Email JSON
- SQS
- Application (Platform Application)
- Lambda
- SMS

Subscription Protocol | Endpoint Type | Example
------------ | -------------
HTTP | An HTTP URL | `http://slappforge.com`
HTTPS | An HTTPS URL | `https://slappforge.com`
Email | A valid email address | `info@slappforge.com`
Email JSON | A valid email address | `info@slappforge.com`
SQS | ARN of a SQS queue | `arn:aws:sqs:us-east-1:111111111111:my-queue`
Application (Platform Application) | ARN of a SNS Platform Application | `arn:aws:sns:us-east-1:111111111111:app/GCM/my-app`
Lambda | ARN of a Lambda function | `arn:aws:lambda:us-east-1:111111111111:function:my-function`
SMS | A valid phone number | `+94123123123`

Once the subscription protocol and endpoint are defined, click the + button to add the subscription.

---

## SNS for Operations
