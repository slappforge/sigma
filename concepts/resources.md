# Resources

In Sigma IDE, a **Resource** represents an instance of a component type, such as a S3 bucket, an API Gateway endpoint, etc. Also a resource can represent an already existing
component or a newly defined one.

Depending on the type of the resource, it can be used either as a trigger, or for operations or for both. Following is a list of currently available resource types and their possible usages.

## AWS Resources

| Resource Type | Use as a Trigger    | Use for Operations
| ---           | :---:               | :---:
| API Gateway   | :white_check_mark:  | :x:
| CloudWatch    | :white_check_mark:  | :x:
| DynamoDB      | :x:                 | :white_check_mark:
| Kinesis       | :white_check_mark:  | :white_check_mark:
| RDS           | :x:                 | :white_check_mark:
| S3            | :white_check_mark:  | :white_check_mark:
| SNS           | :white_check_mark:  | :white_check_mark:
| SQS           | :x:                 | :white_check_mark:


## Reusing a resource

When a resource is defined in Sigma as a trigger or for an operation, it will be listed
under its resource type on the left resource panel.

<p align="center">
  <img width="400" src="./images/resource_list.png">
</p>

Such an already defined resource can be reused for another operation/trigger by directly dragging it to the editor instead of dragging the root resource type.

## Modifying a resource

Depending on the resource type, configurations of some resources can be modified
through the resource panel. Such editable resources have an edit button in-front of them, and the resource configuration panel can be opened by clicking on that button.

<p align="center">
  <img width="400" src="./images/resource_edit.png">
</p>
