# Triggers

Triggers are entry points for your serverless function,
which allow it to be invoked as responses to certain events such as HTTP requests,
scheduled or action-based events (e.g. [CloudWatch Events](../components/aws/cloudwatch.md)),
reception of [stream data](../components/aws/kinesis.md) and so forth.

As serverless functions are event-driven by nature, unless you define one or more triggers for your function,
it would simply sit idle, and will only respond to direct invocations
(e.g. [`Invoke` API calls in case of AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html)).

## Design

In Sigma, a trigger is always associated with a [resource](resources.md).
The same resource may be associated with multiple triggers
(and multiple [operations](operations.md) as well, if the resource type also supports operations).
The associated resource may be either a [new](resources.md#new) one or an [existing](resources.md#existing) one,
but the trigger definition will always be new.

Usually you do not have to worry too much about such particulars,
as Sigma is designed to automatically handle resources and their interdependencies,
but they may be useful when you are trying to troubleshoot any misconfigurations or deployment errors.

## Supported triggers

Sigma currently supports the following types of triggers:

### AWS

- [API Gateway](../components/aws/apig.md)
- [CloudWatch Events](../components/aws/cloudwatch.md)
- [S3](../components/aws/s3.md)
- [Kinesis Streams](../components/aws/kinesis.md)
- [SNS Topics](../components/aws/sns.md#sns-as-a-trigger)

## Managing triggers in Sigma

### <a name="define-trigger"> Defining a trigger

You can configure a trigger for a serverless function by dragging an event source ("base" resource)
from the **Resources** pane on the left, on to the `event` variable on the function header.
Resource types that can provide trigger functionality will be marked with a lightning bolt symbol
(or a composite symbol that contains a lightning bolt) on the bottom right corner of their icon.

<p align="center">
  <img src="./images/symbol-trigger-only.png" alt="A resource type offering exclusive trigger functionality">
  <img src="./images/symbol-trigger-operation.png" alt="A resource type that can provide both trigger and operation functionality">
</p>

As soon as you perform the drag-and-drop, Sigma will open a trigger configuration pop-up
so that you can provide any custom parameters that are required by your trigger.

If you already have a resource definition for the event source entity
(e.g. an [SNS topic](../components/aws/sns.md) or a [Kinesis stream definition](../components/aws/kinesis.md)),
you can directly drag-and-drop it (as above) and specify only the trigger-specific parameters.

If you are configuring a trigger for a source entity that does not already exist in your account
(e.g. a new S3 bucket that is to be created specifically to your application),
you can directly drag-and-drop the resource type entry (e.g. "S3") from the left pane,
upon which Sigma would allow you to define the new resource entry and configure the desired trigger parameters at the same time.
Once a new resource is defined in this manner (as part of a trigger declaration),
it will become available in the left pane and can be used for other triggers and code-level [operations](operations.md)
(API invocations), just like any other resource definition.

### <a name="edit-trigger"> Editing a trigger

After a trigger has been inserted, in addition to modifying the source entity via a [resource edit](resources.md#edit),
you can also modify trigger-specific parameters via the **trigger edit** feature.

Existing triggers of a serverless function can be viewed via the **trigger indicator**,
which appears on the left side pane of the editor window as a lightning bolt icon against the function header.
If the function has no triggers configured yet, the icon will be **red**; otherwise it would be **green**.

<p align="center">
  <img src="./images/no-trigger.png" alt="A lambda function without any configured triggers">
</p>

<p align="center">
  <img src="./images/has-trigger.png" alt="A lambda function with trigger(s)">
</p>

If the function has only one trigger,
clicking the trigger indicator would directly open the edit pop-up for the particular trigger.

<p align="center">
  <img src="./images/trigger-edit.png" alt="Trigger edit pop-up">
</p>

In case of multiple triggers, Sigma would display a trigger selection pop-up
from which you can click the desired trigger for editing.

<p align="center">
  <img src="./images/trigger-selection.png" alt="Trigger selection pop-up">
</p>

While options available under the trigger edit mode are very much component-specific,
in general you would be able to select a different event source entity and modify the trigger-specific parameters.
Usually you will not be able to edit the resource-specific parameters (e.g. shard count of a Kinesis stream);
single-point edits are not allowed for reusable resources (although they can be [globally edited](resources.md#edit)),
since they may be shared across different triggers and operations across multiple lambdas.

### <a name="delete-trigger"> Deleting a trigger

If you no longer require a particular event source to invoke your serverless function,
simply deleting the corresponding trigger will remove the event source association from the next deployment onwards.

Deleting the trigger does not affect the base resource;
it will still be available for use in other triggers and operations inside your Sigma project.

If your lambda has only one trigger, it can be removed by clicking the "Remove" button on the trigger edit pop-up,
and clicking "Yes" on the resulting confirmation dialog.

<p align="center">
  <img src="./images/delete-trigger-single.png" alt="Delete button: single trigger">
</p>

In case of multiple triggers, you can either click the delete button (trash can icon) on the trigger selection pop-up,
or click the desired trigger and use the abovementioned "Remove" button.

<p align="center">
  <img src="./images/delete-trigger-multiple.png" alt="Delete button: multiple triggers">
</p>
