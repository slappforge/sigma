---
description: Process and push messages on scalable AWS SQS cloud message queues, with your serverless functions in SLAppForge Sigma cloud IDE
---

# ![](images/sqs/sqs_icon.svg) AWS SQS (Simple Queue Service)

Amazon Simple Queue Service (SQS) is a fully managed, reliable and highly scalable cloud message queuing service that 
can be used to store and consume messages to keep modularity between application components in microservices and 
distributed systems.
SQS supports both standard and FIFO queue types, to facilitate **at-least-once** and **exactly-once guarantees** 
respectively, while providing highly concurrent access to messages and high availability for producing and consuming 
messages.

The Sigma IDE currently supports most of the available features of SQS service including,
- Standard and FIFO queue types
- Queue level and per message configurations and validations for all provided features like visibility time, delivery
 delay, etc.
- Put, Get and Get and Remove operations are provided.
- Operation wise parameter configuration and validation.

## SQS for Operations
Sigma IDE provides the ability to execute **Get**, **Put** and **Get and Remove** operations on SQS resources. Users 
have the ability to either select an existing queue or configure a new queue to execute the required operation. Sigma 
provides the ability to configure both operation and queue resource under a single window without any overhead to the 
user.

To use SQS resource for operations, it should be dragged from the resources panel and dropped on the required line of 
the lambda code editor. Then the user should select either **Existing Queue** tab or **New Queue** tab based on the 
requirement.

### Using Already Existing SQS Queue

If it's required to execute an operation on a queue which already exists, firstly, the user should select the 
**Existing Queue** mode tab. Then existing queue names will be listed in the Queue Name drop-down list and the user can 
easily select the required queue from that drop-down list. To determine whether a queue is **FIFO**, the user can check 
whether the queue name ends with the suffix **.fifo**.

### Using New SQS Queue

If it is required to execute an operation on a newly created queue, firstly, the user should select the **New Queue** 
mode tab. Then the required parameters should be configured for the queue which is going to be created and invoke the 
required operation.

#### Configuring New SQS Resource

SQS queue resource can be easily configured by providing a queue name and the queue type for the basic use case.

<p align="center">
  <img width="400" src="./images/sqs/sqs_new_queue.png">
</p>

If the queue type is **FIFO**, queue name should be suffixed with **.fifo** extension. This suffix will also be counted 
towards the 80-character queue name limit. On the other hand, if the queue type is **LIFO**, user can provide any 
name as the queue name, but without any special characters.

User can configure advanced properties such as message retention period, delivery delay for the queue, etc. by expanding 
advanced properties section by clicking the **Show Advanced** option.

<p align="center">
  <img width="400" src="./images/sqs/sqs_new_queue_advance.png">
</p>

### Using Already Configured Queue for Subsequent Operations

Once the user configured a new queue from the Sigma IDE, it will be considered as an existing queue for subsequent 
operations. 
If the user wants to execute some operation on already configured queue from the IDE, user can drag the SQS resource 
from the resource panel and then select the **Existing Queue** tab and then select the required queue from the queue 
name drop down list. For the clarity, IDE will show the already configured queues with the **(New)**  suffix for the 
queue name. 

As an example, let's assume that the user first creates a queue with name *worker-queue* and then it's required to use 
that queue for some other operation. In that case, the user can select the queue under the queue name list in the 
existing queue mode as below,

<p align="center">
  <img width="400" src="./images/sqs/sqs_select_already_configured_queue.png">
</p>

### Configure SQS Operation

Once the user configures or selects the queue resource for the operation, the user can configure the required operation 
to be invoked on that queue. Currently, Sigma IDE supports frequently used three main commands, **Put**, **Get** and 
**Get and Remove**. As the first step, the user should select the required operation from the operation drop-down list 
of the configuration window.

#### Put Operation

Put operation inserts a message to the specified queue with the configured payload and attributes. Based on the configured 
queue type, put operation parameters will be changed.

For standard queue (LIFO) type, parameters will be the following.

Field         | Required          | Description
---           | :---:             | :---  
Message Payload     | :white_check_mark:| Payload of the message to be sent
Message Attributes  | :x:         | Key value list of message attributes to be sent with the message

For FIFO queue type, parameters will be the following.

Field                     | Required          | Description
---                       | :---:             | :---
Message Payload           | :white_check_mark:| Payload of the message to be sent.
Message Deduplication Id  | :x:               | The token used for deduplication of sent messages to the queue. If the queue has configured to use *Content-Based Deduplication*, this is optional. Otherwise this is a mandatory parameter.
Message Group Id          | :white_check_mark:| This specifies that a message belongs to a specific message group. Messages that belong to the same message group are processed in a FIFO manner.
Message Attributes        | :x:               | Key value list of message attributes to be sent with the message.


#### Get Operation

Get operation fetches messages from the configured queue. User can configure the following get operation parameters for 
both queue types.

Field                     | Required            | Description
---                       | :---:               | :---  
Maximum Number of Messages| :white_check_mark:  | The maximum number of messages to be returned for the get operation. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values are 1 to 10. Default is 1.
Visibility Timeout For a Message| :white_check_mark: | The duration (in seconds) that the received messages are hidden from subsequent retrieve requests after being retrieved by a ReceiveMessage request.
Wait Time                 | :white_check_mark:  | The duration (in seconds) for which the call waits for a message to arrive in the queue before returning. If a message is available, the call returns sooner than this value.
Message Attribute Names   | :x:                 | The comma separate list of message attribute names to fetch together with each message.
Attribute Names           | :white_check_mark:  | A list of attributes with meta information, that need to be fetched together with each message.

#### Get and Remove Operation

Once a message is read from an SQS queue, that message will not be deleted automatically. It is the consumer's 
responsibility to remove messages once they read the message successfully. To make this process more intuitive and 
user-friendly, Sigma IDE facilitates a custom operation to handle this in a single operation. 

All the parameters for this operation are completely equivalent to the list of parameters in the above Get operation. 
Only the generated code fragments will be different for two operations to handle the difference between the behaviors
 of the two operations. 
