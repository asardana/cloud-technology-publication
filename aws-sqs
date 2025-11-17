Message-oriented Middleware (MoM) allows various applications and microservices within an enterprise to communicate with each other by sending messages asynchronously. Some of the MoMs traditionally used by enterprises are Websphere MQ, Active MQ, TIBCO, Rabbit MQ etc.

Message Queue is used for point to point communication between the message producer and message consumer.

AWS Simple Queue Service (SQS) is a message queue service in the cloud that can be used to store messages from a message producer while they wait to be processed by message consumer. AWS SQS is a distributed queue system which can scale seamlessly to allow messages to be stored temporarily before they are processed by the  message consumer.

Queuing system allows the producers and consumers of the messages to be decoupled from each other and have an asynchronous communication that is highly available, reliable, durable and fault tolerant. Adding a queue in front of a service or an application can make the service/application highly available to the message producer by allowing the messages to be queued during times where service/application might not be available. SQS can support a very large message throughput rate. Message producer can produce messages at a rate faster than the rate at which the message consumer can consume the message. However, care should be taken while designing the queue systems to have queues with sufficient queue depth and to avoid the infinite queue scenarios. You can read more about message queuing theory here.

SQS messages have a visibility timeout which determines how long the messages will be visible in the queue. Once the message is polled by the message consumer, visibility timeout starts and the message is deleted from the queue if the consumer sends a notification for the successful processing of the message before the visibility timeout period ends. If there is no notification by the time visibility timeout expires, message is put back in the queue to allow the consumers to again poll the message for consumption. This mechanism guarantees that the message is processed successfully at least once.

SQS Characteristics
SQS messages can contain up to 256 KB of text in any format, however billed at 64 KB chunks.
SQS allows sending up to 10 messages per batch with a total payload size of up to 256 KB to gain cost efficiency and allowing higher throughput.
Default visibility timeout is 30 seconds and the maximum timeout is 12 hours. The message visibility timeout can be changed by calling the ChangeMessageVisibility action. Visibility timeout of ‘0’ makes the message available for processing immediately.
SQS provides an unlimited backlog of up to 14 days i.e. message retention period can be configured to a value from 1 minute to 14 days. The default value for message retention is set to 4 days.
Queue can be deleted any time, irrespective of whether it’s empty or not.
Queue names are limited to 80 characters and can use alphanumeric, hyphens (-), and underscores (_). Queue names must be unique within an AWS account and can be reused after the queue is deleted.
SQS supports unlimited number of queues and unlimited number of messages per queue for each user.
Amazon CloudWatch can be configured to gather metrics about the queue depth, message rate etc.
SQS Queue Types
Standard Queue- Doesn’t guarantee FIFO and the messages could be delivered more than once. Standard queues provides unlimited throughput.
FIFO Queue – Guarantees that the messages are delivered in the same order in which the messages are put in the queue. Messages are processed only once, hence avoiding duplicates. Message groups allows multiple ordered message streams within a single queue. FIFO queues are limited to 300 messages/second.
SQS Autoscaling
One of the most important features of message queue provisioned in the cloud is the autoscaling capability. Queues can be monitored for certain thresholds that can trigger provisioning of additional computing resources configured in an auto scaling group. Queues can be monitored to increase the number of message consumer instances to allow the consumers to keep up with the rate at which the producers produce the messages.

SQS Long Polling
SQS allows to set a longer polling period of up to 20 seconds in the scenarios where the message rate is low such that consumers polling the queue might not always get a message  to consume thus wasting the computing resources. Longer polling period increases the probability that each polling request will be able to get a message from the queue with a slow message rate when compared to consumers polling up to 20 times per second.

SQS Long Polling wait time can be set at the Queue level or while polling the message from the queue.

Following API actions can be used enable the SQS long polling.

ReceiveMessage – WaitTimeSeconds parameter
CreateQueue – ReceiveMessageWaitTimeSeconds attribute
SetQueueAttributes – ReceiveMessageWaitTimeSeconds attribute
SQS uses short polling when value for WaitTimeSeconds or ReceiveMessageWaitTimeSeconds is set to 0. The wait time parameter while polling the queue has priority over the wait time attribute of the queue.

SQS Rich Client
SQS rich client allows the EC2 instances or other message producers to buffer the messages in a batch allowing higher throughput and lower costs. The application producing the messages does not require any changes to batch the messages and is handled automatically by the rich client. It also allows the incoming messages in a batch to be pre-fetched for consumption reducing the message latency.
