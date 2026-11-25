---
title: "Does SQS FIFO guarantee exactly-once processing?"
weight: 3
chapter: false
menuTitle: "3.3. Blog 3"
---

Imagine Lambda receives `evt-987` for order `1042`, writes it to DynamoDB, then times out before the invocation completes. After the visibility timeout, the same message can become available again. What happens to the order on the second attempt?

**At the queue**, a FIFO queue preserves order within each `MessageGroupId`. If the producer retries with the same `MessageDeduplicationId` within five minutes, SQS suppresses the duplicate send. In this example the producer only sent one message. The second attempt comes from processing that existing message again.

**At the database**, if the whole operation consists of creating one item per event, I can use `eventId` as the partition key and give `PutItem` the condition `attribute_not_exists(eventId)`. The first write creates the item. On a retry of the same event, the condition fails and the original item stays intact. A separate payment or email needs its own way to recognise a retry.

**At the batch level**, with partial batch responses enabled, AWS recommends stopping at the first failure for a FIFO queue and reporting both the failed and the unprocessed messages. A FIFO dead-letter queue can isolate repeated failures. Moving a message aside can change the order in which the business eventually completes the work.

The design question I would ask is concrete: if the function stops immediately after its first successful write, what will the next invocation do to order `1042`?

## AWS documentation

- [FIFO deduplication](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-exactly-once-processing.html)
- [Lambda with SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
- [Lambda batch error handling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html)
- [DynamoDB condition expressions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html)

![Blog Picture]({{< asset "images/blogposted_3.png" >}})
