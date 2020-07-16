---
description: Publish various events to AWS event buses, with AWS EventBridge on Sigma IDE
---

# ![](images/eventbridge/eventbridge_icon.svg) AWS EventBridge

Amazon EventBridge is a serverless event bus that makes it easy to connect applications together using data from your 
own applications, integrated Software-as-a-Service (SaaS) applications, and AWS services. EventBridge delivers a stream 
of real-time data from event sources, and routes that data to targets like AWS Lambda.

Sigma IDE currently supports publishing such events via Lambda functions to one or more Event Buses that exists in your 
AWS account.

> If you need to trigger your Lambda function based on events from an Event Bus, please refer to **Event Pattern Rules**
>in the [CloudWatch Events](./cloudwatch.md#event-pattern-rules) trigger component.

## Publishing events to EventBridge

To publish events to EventBridge from a Lambda function, first drag an EventBridge resource from the resources panel and 
drop on the required line of the lambda code editor. Then on the configuration panel, select **Put Events** as the 
**Operation**.

After that, the **Add Event** button can be used to add one or more events to be published via this operation. Each
such events should have the following fields.

Field       | Required            | Supports Variables  | Description
---         | :---:               | :---:               | ---
Event Bus   | :white_check_mark:  | :x:                 | The event bus that should receive the event
Source      | :white_check_mark:  | :white_check_mark:  | The source of the event
Time        | :x:                 | :white_check_mark:  | The time stamp of the event, per [RFC3339](https://www.rfc-editor.org/rfc/rfc3339.txt). If this is not provided, the time stamp of the `PutEvents` call will be used.
Resources   | :x:                 | :white_check_mark:  | AWS resources, identified by Amazon Resource Name (ARN), which the event primarily concerns
Detail Type | :white_check_mark:  | :white_check_mark:  | Free-form string used to decide what fields to expect in the event detail
Detail      | :x:                 | :x:                 | A valid JSON string that contains the event fields and nested sub-objects

