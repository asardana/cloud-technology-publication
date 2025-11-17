Message-oriented Middleware (MoM) supports a messaging type in which the messages can be broadcasted to multiple message consumers known as message subscribers.

AWS Simple Notification Service is a type of messaging service in the cloud that is based on the pub-sub model. It allows the message publisher to send a message to a Topic which has multiple subscribers that are interested in receiving the same message. The message is delivered to multiple subscribers which can then consume the message to trigger subsequent processes. A Topic is an access point allowing multiple receivers of the message to dynamically subscribe for identical copies of the same notification.

When compared to SQS which is queue with a pull mechanism, SNS is a fan-out with push to send out the messages to subscribers. This eliminates the need for the message consumers to periodically poll for any new messages.

SNS Key Features
Unlimited fan-out to send the message to multiple subscribes simultaneously
Supports multiple protocols – Amazon SQS, HTTP, HTTPS, Email, Email-JSON, SMS, Application, Lambda
Customizable delivery policies like rate limiting and retries
Messages published to SNS topics are stored redundantly across multiple availability zones
Amazon CloudWatch metrics for publication and subscription

Share this:
